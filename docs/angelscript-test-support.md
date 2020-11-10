Title: Angelscript Test Support

# Angelscript Test Support
Angelscript features a xUnit-style unit testing framework. There is also an
integration test framework that can play back game scenarios and wait for
some condition to occur.

# Unit Tests
```jsx
void Test_NameOfTheTestCase(FUnitTest& T)
{
    // Fails the test.
	T.AssertTrue(false);
	T.AssertEquals(1, 1 + 1);
	T.AssertNotNull(nullptr);
}
```

You can put test code in any Angelscript file, but by convention these
are put in File_Test.as if your production code is in File.as.

## Running Unit Tests
Unit tests run on hot reload, so to run a test you just create a test like
above, open the Unreal editor, and save the file. In Unreal, go
Window > Developer Tools > Output Log and you will see lines like

```
Angelscript: [RUN]    Some.Angelscript.Subdir.Test_LaunchesNukesWhenCodesAreEntered
...
Angelscript: [OK]     Some.Angelscript.Subdir.Test_LaunchesNukesWhenCodesAreEntered (0.2530 ms)
```

Furthermore, the tests show up under the category "Angelscript" in the Test Automation tool.
You will need to install that one into Unreal. See [https://docs.unrealengine.com/en-US/Programming/Automation/UserGuide/index.html](https://docs.unrealengine.com/en-US/Programming/Automation/UserGuide/index.html)

You can also run tests from the command line:
```
Engine\Binaries\Win64\UE4Editor-Cmd.exe \Path\To\your.uproject -NullRHI -NoSound -NoSplash -ExecCmds="Automation RunTests Angelscript.UnitTests" -TestExit "Automation Test Queue Empty+Found 0 Automation Tests, based on" -unattended -stdout -as-exit-on-error
```

## Installing a Custom Game Mode for Unit Tests
You can add this line to one of your .ini files in your project to get a game mode in your tests:
You can then create a blueprint at the specified location and put whatever settings you want in there.
This will be used by all unit tests.
[/Script/EngineSettings.GameMapsSettings]
...
+GameModeClassAliases=(Name="UnitTest",GameMode="/Game/Testing/UnitTest/BP_UnitTestGameMode.BP_UnitTestGameMode_C")

# Integration Tests
Each integration test has a map where you can draw up any geometry or place any actors you like.

Add this to for instance MyTestName_IntegrationTest.as:

```jsx
void IntegrationTest_MyTestName(FIntegrationTest& T)
{	
}
```

Then you need to add a test map MyTestName.umap to /Content/Testing/ (create the dir if you don't have it
in your project yet). The map name is always the part after IntegrationTest_ in the test name.

You can retrieve placed actors like this (or spawn them in the test):
```jsx
// Looks up an actor in the map
AActor GetActorByLabel(UClass Class, const FName& Label)
{
#if EDITOR
    TArray<AActor> Actors;
    GetAllActorsOfClass(Class, Actors);

    for (AActor Actor: Actors)
    {
        if (Actor.GetActorLabel() == Label)
        {
            return Actor;
        }
    }

    FString AllActorLabels = "";
    for (AActor Actor: Actors)
    {
        AllActorLabels += "- " + Actor.GetActorLabel() + "\n";
    }
    if (AllActorLabels.IsEmpty()) {
        Throw(
            "Did not find an actor with class " + Class.GetName() +
            " and label " + Label + ". In fact, there no actors in this level.");
    } else {
        Throw(
            "Did not find an actor with class " + Class.GetName() +
            " and label " + Label + ". Found these actors:\n" + AllActorLabels);
    }
#endif  // EDITOR
	Throw("GetActorByLabel is only for testing, i.e. when in EDITOR mode.");
	return nullptr;
}
```

## Latent Automation Commands
The code in the test function executes before the map is loaded and before the first frame executes. The test is not complete when the test function returns therefore, it has merely enqueued a series of *latent automation commands* ([Unreal documentation](https://docs.unrealengine.com/en-US/Programming/Automation/TechnicalGuide/index.html)). If we assume the test enqueues no latent commands of its own (like the one above), the test framework will enqueue the following actions (see IntegrationTest.cpp):

- FWaitForMapToLoadCommand()
- FEnsureWorldLoaded()
- FExitGameCommand()
- FReportFinished()
- FFreeTest()

These execute in sequence. Each action can take multiple engine frames to execute.

The test can enqueue latent commands of its own:

```jsx
void IntegrationTest_AlienShootsRepeatedly(FIntegrationTest& T)
{ 
    AActor A = GetActorByLabel(ABulletSponge::StaticClass(), n"BulletSponge");
    ABulletSponge Floor = Cast<ABulletSponge>(A);

    T.AddLatentAutomationCommand(UGetsShotXTimes(Floor, 2));
}
```

The action is enqueued using `T.AddLatentAutomationCommand`. The set of latent actions will now be:

- ...
- FEnsureWorldLoaded()
- UGetsShowXTimes()
- FExitGameCommand()
- ...

AddLatentAutomationCommand takes a `ULatentAutomationCommand`:

```jsx
UCLASS()
class ABulletSponge : AStaticMeshActor
{
    int NumTimesHit = 0;

    UFUNCTION(BlueprintOverride)
    void BeginPlay()
    {
        OnTakeAnyDamage.AddUFunction(this, n"TakeAnyDamage");
    }

    UFUNCTION()
    void TakeAnyDamage(AActor DamagedActor, float Damage, const UDamageType DamageType, AController InstigatedBy, AActor DamageCauser)
    {        
        NumTimesHit++;
    }
}

class UGetsShotXTimes : ULatentAutomationCommand
{
    private ABulletSponge BulletSponge;
    private int ExpectedNumHits;

    UGetsShotXTimes(ABulletSponge Target, int X)
    {
        BulletSponge = Target;
        ExpectedNumHits = X;
    }

    UFUNCTION(BlueprintOverride)
    bool Update()
    {
        return BulletSponge.NumTimesHit > ExpectedNumHits;
    }

    UFUNCTION(BlueprintOverride)
    FString Describe() const
    {
        return BulletSponge.GetPathName() + ": bullet sponge got hit " + BulletSponge.NumTimesHit + "/" + ExpectedNumHits;
    }
}
```

The game engine will keep ticking as long as Update returns false. This means you can wait on any condition you can think of. The default timeout is five seconds though, so you can't wait for too long.

You can specify `default bAllowTimeout = true` on a latent command to allow it to time out. This is useful if you want to test that something is *not* happening
(e.g. check actor doesn't move out of bounds during 5 seconds).

## Running Integration Tests
These don't run on hot reload like unit tests, so you need to invoke them through the Test Automation window in Unreal. They are run just like unit tests, see above.

To run integration tests from the command line, run the same line as for unit tests but replace Angelscript.UnitTests with Angelscript.IntegrationTests.