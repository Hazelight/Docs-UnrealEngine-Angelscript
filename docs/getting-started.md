Title: Getting Started

# Getting Started

## Basics
Start the unreal editor by executing `Engine/Binaries/Win64/UE4Editor.exe`.

Create a new project or open an existing project and convert it to use the editor version used
by UnrealEngine-Angelscript.

Start [Visual Studio Code](https://code.visualstudio.com/), then open the `File > Open Folder` dialog.
Browse to the location of your unreal engine project (Usually `Documents/Unreal Projects/MyProjectName`),
then select and open the `Script/` directory within it.

The editor will automatically look for new files with the `.as` extension within 
your project's `Script/` directory and load them when they are added or changed.

## Script Examples
Several example script files are included in the UnrealEngine-Angelscript repository.

You can open the `Script-Examples/` folder with the engine version you downloaded,
and copy the `Examples/` folder within that into your project's `Script/` directory.

Good starting points for reading the examples are `Example_Actor.as`
and `Example_MovingObject.as`.

## Example Actor Class
```
/*
 * This is an example of an object that simply moves left
 * and right over time. Several features of script classes
 * are used here, and this is an easy way to demonstrate
 * hot reload as well by changing the behavior.
 */

class AExampleMovingObject : AActor
{
	/* Properties set to DefaultComponent will be
	   created as components on the class automatically,
	   without it, it would just be a reference to any
	   component. Setting RootComponent makes this component
	   the default root for this class. */
	UPROPERTY(DefaultComponent, RootComponent)
	UStaticMeshComponent Mesh;
	default Mesh.StaticMesh = Asset("/Engine/BasicShapes/Cone.Cone");

	/* The 'default' keyword is used to set properties on subobjects or
	   parent classes. This is equivalent to setting them in the constructor in C++. */
	// Default value for property on component:
	default Mesh.RelativeLocation = FVector(0.f, 0.f, 0.f);
	// Default value for property on parent class:
	default bReplicates = true;

	/* Setting the Attach specifier allows you to determine
	   where a default component gets attached in the hierarchy
	   without having to code it manually. */
	UPROPERTY(DefaultComponent, Attach = Mesh)
	UBillboardComponent Billboard;
	default Billboard.SetHiddenInGame(false);

	UPROPERTY()
	float MovementPerSecond = 100.f;

	/* Properties can be edited on the instance by default unless NotEditable is specified. */
	UPROPERTY(NotEditable, BlueprintReadOnly)
	FVector OriginalPosition;

	/* Not all properties need to be known by unreal. */
	bool bHeadingBack = false;

	/* We can override beginplay to execute script when the actor enters the level. */
	UFUNCTION(BlueprintOverride)
	void BeginPlay()
	{
		// Record the position of the object on BeginPlay so we know where to go back to.
		OriginalPosition = GetActorLocation();
	}

	/* Override the tick function to do the actual movement logic. */
	UFUNCTION(BlueprintOverride)
	void Tick(float DeltaSeconds)
	{
		FVector NewLocation = GetActorLocation();
		if (bHeadingBack)
		{
			NewLocation -= FVector(DeltaSeconds * MovementPerSecond, 0, 0);

			// Reverse after moving a certain amount in X
			if (NewLocation.X < OriginalPosition.X - 100)
				bHeadingBack = false;
		}
		else
		{
			NewLocation += FVector(DeltaSeconds * MovementPerSecond, 0, 0);

			// Uncomment to zigzag a little bit
			//NewLocation += FVector(0, 100 * DeltaSeconds, 0);

			// Reverse after moving a certain amount in X
			if (NewLocation.X > OriginalPosition.X + 100)
				bHeadingBack = true;
		}

		// Uncomment and save to teleport the actor back to its original position!
		//NewLocation = OriginalPosition;

		// Arguments can be passed by name, allowing them to be out of order
		SetActorLocation(NewLocation, bTeleport = true, bSweep = false, SweepHitResult = FHitResult());
	}
};
```
