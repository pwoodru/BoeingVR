# s21-boeing

Video Presentation of Final Product: https://www.youtube.com/watch?v=k1Sw0iN0oIY

Technical Documentation

## Main Menu Scripts:

### ChangeScene.cs

This script is used on the scene changing buttons of the Main Menu’s tablet to load the user into their selected scene. If the user hits the “Main Menu” button, the function “ChangeToMenu()” is called to send them to the main menu. If the user hits the “SandboxTutorial” button, the function “ChangeToSandbox()” is called to send them to the sandbox scene. Lastly, if the user hits the “Tooling Practice” button, the function “ChangeToTooling()” is called to send the user to the ToolingEnvo scene.

ChangeScene.cs:

    using System.Collections;
    using System.Collections.Generic;
    using UnityEngine;
    using UnityEngine.SceneManagement;

    public class ChangeScene : MonoBehaviour
    {
        //Provided the string for the scene name:
        //Change to sandbox
        public void ChangeToSandbox() {
            SceneManager.LoadScene("SandboxTutorial");
        }
        //Change to tooling station
        public void ChangeToTooling() {
            SceneManager.LoadScene("ToolingEnvo");
        }
        //Change to main menu
        public void ChangeToMenu() {
            SceneManager.LoadScene("MainMenu");
        }
    }
 
## Sandbox Tutorial Scripts:

### TaskCycle.cs

TaskCycle.cs is the script that handles the task rotation accessed by the “Task” button of the SandboxTutorial’s tablet. This window shows the user every task available to them, allows them to cycle to each task, provides a complete/incomplete button, and provides a “Back” button to return to the menu. The script only handles which task is shown to the user and the buttons that transition between tasks.
To start, the script initializes the task number to “1”, activates task number 1, and deactivates the rest of the tasks.
 
Upon the user hitting either the “back” button or the “next” button, the script will then decrement the task number by 1 for hitting back, or increment the task number by 1 for hitting next. If the value of task number goes below 1, task number will be manually set to 6, the last task page, for cycling purposes. The same goes for the task number going above 6, the task number will be set to 1, the first task page, for cycling purposes. Each time the user hits back or next, the script will first deactivate the currently shown task window and then activate the new task window along with any buttons necessary for its functionality.

TaskCycle.cs:

    using System.Collections;
    using System.Collections.Generic;
    using UnityEngine;

    public class TaskCycle : MonoBehaviour
    {
    public GameObject t1;
    public GameObject t2;
    public GameObject t3;
    public GameObject t4;
    public GameObject t5;
    public GameObject t6;
    public GameObject completeButton;
    public GameObject sandboxButton;
    public int taskNum;

    void Start() {
        taskNum = 1;
        t1.SetActive(true);
        t2.SetActive(false);
        t3.SetActive(false);
        t4.SetActive(false);
        t5.SetActive(false);
        t6.SetActive(false);
        completeButton.SetActive(true);
        sandboxButton.SetActive(false);
    }

    public void NextTask() {
        taskNum++;
        if (taskNum > 6) {
            taskNum = 1;
        }
        if (taskNum == 1) {
            t6.SetActive(false);
            sandboxButton.SetActive(false);
            t1.SetActive(true);
        }
        if (taskNum == 2) {
            t1.SetActive(false);
            t2.SetActive(true);
        }
        if (taskNum == 3) {
            t2.SetActive(false);
            t3.SetActive(true);
        }
        if (taskNum == 4) {
            t3.SetActive(false);
            t4.SetActive(true);
        }
        if (taskNum == 5) {
            t4.SetActive(false);
            sandboxButton.SetActive(false);
            t5.SetActive(true);
        }
        if (taskNum == 6) {
            t5.SetActive(false);
            completeButton.SetActive(false);
            t6.SetActive(true);
        }
    }

    public void BackTask() {
        taskNum--;
        if (taskNum < 1) {
            taskNum = 6;
        }
        if (taskNum == 1) {
            t2.SetActive(false);
            sandboxButton.SetActive(false);
            t1.SetActive(true);
        }
        if (taskNum == 2) {
            t3.SetActive(false);
            t2.SetActive(true);
        }
        if (taskNum == 3) {
            t4.SetActive(false);
            t3.SetActive(true);
        }
        if (taskNum == 4) {
            t5.SetActive(false);
            t4.SetActive(true);
        }
        if (taskNum == 5) {
            t6.SetActive(false);
            sandboxButton.SetActive(false);
            t5.SetActive(true);
        }
        if (taskNum == 6) {
            t1.SetActive(false);
            completeButton.SetActive(false);
            t6.SetActive(true);
        }
    }
    
    }


### SnapCollider.cs
   
SnapCollider.cs is the script that allows grabbable objects to be dropped and locked into a certain position. This function takes in 4 primary parameters that can be set via the Unity Editor: tag, spot_x, spot_y, spot_z. This allows the developer to determine what type of objects can be placed into the snap collider (only objects whose tag matches "tag" will lock into place). It also allows you to specify the precise position where the object will snap into place.

To use SnapCollider, simply drop an object into the area in the object's collider. If there is any collision with a valid object inside the SnapCollider's collision area, this script will check whether or not the object is being grabbed. If the object is not currently being grabbed, it will snap into the specified location.

SnapCollider.cs:

      using System.Collections;
      using System.Collections.Generic;
      using UnityEngine;

      public class SnapCollider : MonoBehaviour
      {
          public string cubeTag;
          [SerializeField]
          private Vector3 spot = new Vector3(0,0,0);
          public bool objectInPlace = false;
          public bool active = true;
          //public TimerManager timer;

          private void OnTriggerEnter(Collider other) {
              if(active) {
                  if(other.tag == cubeTag) {
                      if(other.GetComponent<OVRGrabbable>().isGrabbed == false && other.GetComponent<Rigidbody>().useGravity == true) {
                          //other.transform.position = new Vector3(-4.04f, 1.0f, -1.72f);
                          other.transform.localPosition = spot;
                          other.transform.rotation = Quaternion.identity;
                          other.attachedRigidbody.velocity = Vector3.zero;
                          other.attachedRigidbody.angularVelocity = Vector3.zero;
                          other.attachedRigidbody.useGravity = false;
                          objectInPlace = true;

                          //timer.Reset();
                      }
                  }
              }

          }

          private void OnTriggerExit(Collider other) {
              if(active) {
                  if(other.tag == cubeTag) {
                      other.GetComponent<Rigidbody>().useGravity = true;
                  }
              }
          }
      }



## Tooling Station Scripts:



## Desktop Application Scripts:

Certain scripts apply solely to the desktop application, to apply certain functionality in the desktop application that relies on the Oculus Integration package, or OVR. Rather than use OVR-dependent scripts for the desktop application, we needed to use a library called Tilia, which has the packages for our Spatial Simulator and other interactable objects. Because Tilia was the main library for desktop integration, scripts that were adapted from OVR functionality to Tilia functionality are denoted with "\_tilia" at the end of the script name. 

These scripts are: 
 - SnapCollider_tilia.cs
 - ChangeScene_tilia.cs
 - Drill_tilia.cs
 - NailGun_tilia.cs
 - TorqueWrench_tilia.cs

Each of these scripts follows similar paradigms as the original OVR scripts, and copy a large part of the original OVR code. The main distinguishing factor between OVR-dependent scripts and Tilia-dependent scripts is the swapping between OVRGrabbables & InteractableFacade (Tilia) objects. There are other small changes made due to differences in functionality between OVR and Tilia. To understand how to swap a scene's functionality between OVR and Tilia, see HubbarthArtifact10 in the Research folder.

## UI Scripts:
> Objects in this category are intended to display visual affects to the user without directly interacting with physics in the scene.

### Display.cs
> Display is a static class containing 4 visual effects in addition to the the window system. These visual effects can be created without a corresponding Unity GameObject or with them using their "Action" variants.

This class is referenced by the Toast, Annotation, Indicator, Ruler, and window scripts to place content relative to where the user is located and looking. Any of these visual effects will also be located in the VRUI layer, as to not interface with physics objects in the scene.

```cs
/// <summary>Container for a variety of visual effects</summary>
public static partial class Display {
	/// <summary>
	/// Transform of the object used to view the scene
	/// 
	/// This is typically the main camera
	/// </summary>
	public static UnityEngine.Transform Transform { get; set; }

	/// <summary>Layer which all UI objects are placed in</summary>
	public const string LayerName = "VRUI";

	/// <summary>Layer which all UI objects are placed in</summary>
	public static int Layer => LayerMask.NameToLayer(LayerName);
}
```

### Toast.cs

The toast class can be used to display textual information in the users periphery to notify them. A toast will smoothly follow after the user if they move but does not remain in front of them if they turn. When multiple toasts are shown at once, they will be layered above each other and smoothly disappear based on their duration through an closing animation.

It can be utilized by creating a new instance using the constructor. The visual appearance can be adjusted using the fields and properties available in the class. Since the toast uses [TextMesh Pro](https://learn.unity.com/tutorial/textmesh-pro-style-sheets-and-styles#5f3f63beedbc2a176bb3ff97) for creating text, images and a variety of text formatting options may be used as well. A toast can additionally be destroyed before its duration is over by calling the `Cancel()` method.

For example, creating a notification with blue text saying "Hello, world!" would be: `new Toast("Hello, world!").TextColor = Color.blue`.

```cs
public static partial class Display {
	/// <summary>
	/// Textual feedback indicator
	/// 
	/// Also referred to as notifications
	/// </summary>
	public class Toast {
		/// <summary>How long the visual effect lasts</summary>
		public readonly Duration duration;

		/// <summary>Text displayed</summary>
		[Percept.DefaultValue("Sample text")]
		public string Message { get; }

			/// <summary>Text displayed</summary>
		[Percept.DefaultValue("Sample text")]
		public string Message { get; set; }

		public Color TextColor { get; set; }

		public Color BackgroundColor { get; set; }

		public Toast(string message = null, Duration duration = Duration.Medium);

		/// <summary>Opacity of the text and background</summary>
		[Percept.Range(0, 1)]
		[Percept.DefaultValue(1)]
		public float Opacity { get; set; }

		/// <summary>Size of the text and background</summary>
		[Percept.DefaultValue(1)]
		public float Size { get; set; } = 1f;

		/// <summary>Distance to maintain between the target object</summary>
		[Percept.DefaultValue(1)]
		public float Distance { get; set; } = 1f;

		/// <summary>Destroys the toast prematurely</summary>
		public void Cancel();
		
		/// <summary>Indicates the amount of time a toast will last</summary>
		public enum Duration {
			/// <summary>1 second</summary>
			VeryShort,
			/// <summary>1.5 seconds</summary>
			Short,
			/// <summary>2.25 seconds</summary>
			Medium,
			/// <summary>3 seconds</summary>
			Long,
			/// <summary>3.5 seconds</summary>
			VeryLong,
			/// <summary>Remains until manually destroyed</summary>
			Indefinite
		}
	}
}

/// <summary>An annotation that is only shown when hovered by a pointer</summary>
[RequireComponent(typeof(Interactor))]
public class AnnotateAction : Annotate {}
```

### Annotation.cs

Annotations display textual information similar to toasts. However, annotations are bound to objects other as opposed to be shown directly to the user.

Unlike toasts, annotations require some set up in the scene before they can be used. They will need to be passed a GameObject that they are being used to annotate. Additionally, they will need a means to change the Focused property, which determines whether the annotation should be shown and is false by default.

To create a sphere with the annotation "This is a sphere.", you could use the following: `new Annotation(GameObject.CreatePrimitive(PrimitiveType.Sphere), "This is a sphere.")`.

```cs
public static partial class Display {
	public class Annotation {
		/// <summary>Object being annotated</summary>
		public readonly GameObject target;

		/// <summary>Position of the annotation relative to the target</summary>
		[Percept.DefaultValue(Anchor.Above)]
		public Anchor anchor = Anchor.Above;

		/// <summary>Angle to rotate the annotation about the y-axis from the target's rotation</summary>
		public float angleOffset;

		/// <summary>Additional offset distance in the direction of the anchor relative to the target</summary>
		[Percept.DefaultValue(0.1f)]
		public float distanceOffset = 0.1f;

		public Annotation(GameObject target, string text = null);

		/// <summary>Text to display that describes the target object</summary>
		public string Text { get; set; }

		/// <summary>
		/// Width of the text
		/// 
		/// If zero or nonpositive, the width will adapt to the rendered width of the target object
		/// </summary>
		public float Width { get; set; }

		/// <summary>Size of the text</summary>
		[Percept.DefaultValue(2f)]
		public float Size { get; set; }

		/// <summary>Text color</summary>
		[Percept.ColorUsage(true)]
		public Color Color { get; set; }

		/// <summary>Whether the annotation is in focus and should be shown</summary>
		public bool Focused { get; set; }

		/// <summary>Removes the annotation</summary>
		public void Remove();

		/// <summary>Where the annotation is placed relative to the target</summary>
		public enum Anchor {
			Left,
			Right,
			Above
		}
	}
}

/// <summary>An annotation that is only shown when hovered by a pointer</summary>
[RequireComponent(typeof(Interactor))]
public class AnnotateAction : Annotate {}
```

### Indicator.cs

This class, though unused at our current iteration in the project, can portray the exact position of an object and give the user a direction as to where the object is located.

It does this by highlighting the specified object in a color and displaying an arc below the user pointing in the direction of the object. This ground arc will contract when the object is further and expand when the object is closer. It will also tilt slightly up or down depending on how high the object is from the user's head height.

For example, indicating an object named "Objective" in the color yellow could be done with the following: `new Indicator(GameObject.Find("Objective"), Color.yellow)`.

```cs
public static partial class Display {
	/// <summary>Coaxes the user to look in a particular direction and at particular object</summary>
	public class Indicator {
		/// <param name="target">Object to coax the user to look at</param>
		public Indicator(GameObject target, Color iconColor, Color highlightColor);

		public Indicator(GameObject target, Color color, float intensity = 1) : this(target, color, color * intensity) {}

		/// <summary>Minimum indicator width at closest distance to target</summary>
		[Percept.DefaultValue(1f)]
		public float MinSize { get; set; } = 1f;

		/// <summary>Maximum indicator width at furthest distance from target</summary>
		[Percept.DefaultValue(4f)]
		public float MaxSize { get; set; } = 4f;

		/// <summary>Maximum angle in degrees for the indicator to point towards the target from the display</summary>
		[Percept.Range(0, 90)]
		[Percept.DefaultValue(45f)]
		public float MaxTurnAngle { get; set; } = 45f;

		/// <summary>Maxmimum angle in degrees for the indicator to pitch towards the target from the display to represent a height difference</summary>
		[Percept.Range(0, 45)]
		[Percept.DefaultValue(5f)]
		public float MaxPitchAngle { get; set; } = 5f;

		/// <summary>How quickly the indicator rotates to the correct angle</summary>
		[Percept.DefaultValue(10f)]
		public float Speed { get; set; } = 10f;

		/// <summary>Color of the directional indicator object</summary>
		[Percept.ColorUsage(true, true)]
		public Color IconColor { get; set; }

		/// <summary>Color to highlight the target object with</summary>
		public Color HighlightColor { get; set; }

		/// <summary>Removes the highlight color and indicator object</summary>
		public void Cancel();
	}
}
```

### Ruler.cs

The ruler is a visual tool that can be used to determine accurate, real world measurements for objects within the virtual environment.

The primary components of the ruler are the start and end points, which dictate from where and to where the ruler stretches.

Visual aspects of the ruler consist of the unit system displayed and whether marks are shown on both sides. These two attributed can be modified using the `Marks` attribute for unit system usage and `DoubleSided` to change whether marks are shown on both sides.

One more notable feature is that any point along the ruler can be measured textually using the `Measure(Vector3)` function and passing in a point along the path of the ruler. This point may be acquired through raycasting at the ruler on the "VRUI" layer.

For example, displaying a ruler between the center's of the objects named "A" and "B" would be the following: `new Ruler(GameObject.Find("A").transform.position, GameObject.Find("B").transform.position)`.

```cs
public static partial class Display {
	/// <summary>Used to measure distance between points in the scene with accurate units</summary>
	public class Ruler {
		/// <summary>Acquires all rulers present in the scene</summary>
		public static Ruler[] All { get; }

		/// <param name="gameObject">Base object for the ruler</param>
		/// <param name="start">Start point in global space</param>
		/// <param name="end">End point in global space</param>
		public Ruler(GameObject gameObject, Vector3 start, Vector3 end);

		public Ruler(Vector3 start, Vector3 end);

		/// <summary>Creates a ruler from the display's position to the endpoint</summary>
		/// <param name="end">End point in global space</param>
		public Ruler(Vector3 end);

		/// <param name="gameObject">Base object for the ruler</param>
		public Ruler(GameObject gameObject);

		public Ruler();

		/// <summary>Start point</summary>
		public Vector3 Start { get; set; }

		/// <summary>End point</summary>
		public Vector3 End { get; set; }

		/// <summary>How wide the ruler is visually</summary>
		[Percept.DefaultValue(0.2f)]
		public float Width { get; set; }

		/// <summary>Unit system to use for tick marks</summary>
		[Percept.DefaultValue(UnitSystem.Metric)]
		public UnitSystem Marks { get; set; }

		/// <summary>Whether the tick marks are shown on both sides</summary>
		public bool DoubleSided { get; set; }

		/// <summary>Whether the ruler can be seen</summary>
		[Percept.DefaultValue(true)]
		public bool Visible { get; set; }

		/// <param name="point">Point on the ruler's surface</param>
		/// <returns>The distance along the ruler of the point</returns>
		public float Measure(Vector3 point);

		/// <summary>Removes the ruler from the scene</summary>
		public void Remove();

		/// <returns>Whether the specified GameObject is a ruler</returns>
		public static bool Is(GameObject gameObject);

		/// <param name="ruler">Ruler instance of the GameObject</param>
		/// <returns>Whether the specified GameObject is a ruler</returns>
		public static bool Is(GameObject gameObject, out Ruler ruler);

		public enum UnitSystem {
			Metric,
			Imperial
		}
```

## UI Scripts (Window System):
> Objects in this category are still classified as UI objects but are distinct from other visual effects since they are bound to a Window object.

> The Window System operates on a prefab based approach. Windows are designed in an isolated prefab context and placed in the scene once finished. Prefabs placed in the scene should never be modified there. All changes must be done in the prefab editor mode.

To make a window, look to the tab called VRUI. In this tab, press New Window and to be taken into the prefab editor with an empty GameObject including a WindowBase script. Availble elements are shown on the WindowBase script, such as buttons, text, and sliders.

Pressing on any of these elements will create a child GameObject with a corresponding script derived from `Element` and select it. You can then modify the child object's properties to change the appearance and functionality.

To spawn the window, call `Display.Window("<path>")`. The path is implicitly within "Assets/Resources/Prefabs/Windows". Assuming you didn't change the name of the prefab, you can use "New Window" as the path.

### Window.cs

This class is used to programtically create windows from scripts. It also offers a variety of parameters to determine where the window will be placed relative to the user.

```cs
public static partial class Display {
	/// <summary>Spawns a window in an orientation at a position</summary>
	/// <param name="path">Path to the window prefab</param>
	/// <param name="position">Global position to spawn the window at</param>
	/// <param name="rotation">
	/// Global orientation to spawn the window in
	/// 
	/// If unspecified, the window will face the display
	/// </param>
	/// <returns>The base component of the spawned window</returns>
	public static WindowBase Window(string path, Vector3 position, Quaternion? rotation = null);

	/// <summary>Spawns a window in front of the display</summary>
	/// <param name="path">Path to the window prefab</param>
	/// <param name="distance">Distance from the display</param>
	/// <returns>The base component of the spawned window</returns>
	public static WindowBase Window(string path, float distance = 0.6f);
}
```

### WindowBase.cs

The core of the window system. All window Elements must be a child to a WindowBase object within the scene hierarchy.

When implementing features that involve navigating between different menus in a single window, use the `Replace(string)` function to maintain window position and orientation in the world.

```cs
/// <summary>Used to represent an object with the sole purpose of displaying window elements to creaste some user interface</summary>
public class WindowBase : MonoBehaviour {
	/// <summary>Where to find window presets in assets</summary>
	public const string Location = "Prefabs/Windows";

	/// <summary>How physically thick the window and its elements are</summary>
	public float thickness = 0.025f;

	/// <summary>Called when this window is closed</summary>
	public Action onClose;

	/// <summary>Called when this window is replaced by another</summary>
	public Action<WindowBase> onReplace;

	/// <summary>Closes this window</summary>
	public void Close();

	/// <summary>Replaces this window with the specified one</summary>
	/// <param name="path">Path to window prefab</param>
	public WindowBase Replace(string path);
}
```

### Element.cs

This base class contains many of the methods used to provide functionality for a window. Any of its methods may be accessed through the various element classes that extend it.

One feature of Element to note is the `Replace(string)` method. This function acts as if `Replace(string)` where called on the parented WindowBase object. As such, calling replace on a child element will replace the entire window. This is beneficial since the WindowBase object does not need to be referenced by the Element object in the inspector to use this functionality.

```cs
/// <summary>The base class from which all UI elements meant to be used in a window inherit from</summary>
public abstract class Element : MonoBehaviour {
	/// <summary>Distance from window base plane</summary>
	public float Depth { get; set; }

	/// <summary>Called when depth changes</summary>
	public Action<float, float> onDepthChanged;

	/// <summary>Replaces this window with the specified one</summary>
	/// <param name="path">Path to window prefab</param>
	public void Replace(string path);

	/// <summary>Opens a new window</summary>
	/// <param name="path">Path to window prefab</param>
	public WindowBase Open(string path);

	/// <summary>Opens a new window</summary>
	/// <param name="path">Path to window prefab</param>
	public void OpenVoid(string path);

	/// <summary>Closes the parent window</summary>
	public void Close();

	/// <summary>Toggles whether the element is enabled for interaction</summary>
	public void Toggle();
}
```

* #### ButtonElement.cs
	This element will display a 3d button that can be physically interacted with using the controller.

	A button press can be detected with the `pressed` property, allowing some action to be carried out once it has.

	```cs
	/// <summary>Displays a physically pressable button</summary>
	[ExecuteInEditMode]
	public class ButtonElement : Element {
		[Tooltip("Background color of the button")]
		public Color color = Color.white;

		[Tooltip("Called when the user has fully depressed the button with their controller/hand")]
		public UnityEvent pressed;

		/// <summary>Activates event associated with this button being pressed</summary>
		public virtual void Press();
	```
* #### TextElement.cs
	Simple, readable text.
	
	The position of the text can be adjusted using the transform tool.

	Properties such as font, color, and alignment can be set using the TextMesh Pro component.

	```cs
	/// <summary>Displays text in a window</summary>
	[ExecuteInEditMode]
	public class TextElement : Element {}
	```
* #### ImageElement.cs
	Although images may also be displayed in a TextElement, using ImageElement is much more robust.

	To show an image, simply drag a texture into the texture slot and position the element appropriately within the window.

	```cs
	/// <summary>Displays an image</summary>
	[ExecuteInEditMode]
	public class ImageElement : Element {
		[Tooltip("Tint to give the texture")]
		public Color color = Color.white;

		[Tooltip("Texture shown in the image area")]
		public Texture texture;
	}
	```
* #### BackgroundElement.cs
	This element is similar to ImageElement, however, it grows and shrinks to encapsulate all objects in the window.

	Use this element if you want a background for the window but do not want to manually adjust it when adding/removing other elements.

	```cs
	/// <summary>
	/// Displays a content fitting background texture for the window.
	/// 
	/// Typically, only a single background element is used at the sibling level as it accounts for the size of other siblings from the same parent.
	/// </summary>
	[ExecuteInEditMode]
	public class BackgroundElement : ImageElement {
		[Tooltip("How much to scale the background beyond the size of the content")]
		public float size = 1f;
	}
	```
* #### CheckboxElement.cs
	Checkboxes are used when a feature needs a toggle or a single item must be selected from multiple.

	The status of a checkbox, whether it is ticked or not, may be checked using the 
	`state` field in the inspector or the `State` property in other scripts.

	To make the ticked status of a checkbox mutually exclusive among others, add the alternatives to the checkbox's `siblings` field. This ensures that ticking this checkbox unticks the siblings.

	```cs
	public class CheckboxElement : ButtonElement {
		[Space]
		[Tooltip("Text used when the box is ticked")]
		public string tickText = "O";
		[Tooltip("Text used when the box is not ticked")]
		public string untickedText = "X";

		[Tooltip("Whether the checkbox is currently ticked")]
		[SerializeField]
		private bool state;

		[Space]
		[Tooltip("Checkboxes which must be unticked while this checkbox is ticked")]
		public CheckboxElement[] siblings;

		public bool State { get; set; }
	}
	```
* #### ProgressBarElement.cs
	This element is used to show the percent completion of a task or activity.

	Its appearance can be modified using the color and length/size properties. To define how much the progress bar should be filled, the progress parameter may be specified between 0 and 1.

	For this element, 0 represents 0% completion and 1 represents 100% completion.

	```cs
	/// <summary>Displays a progress bar</summary>
	[ExecuteInEditMode]
	public class ProgressBarElement : Element {
		[Tooltip("Color of the unfilled portion of the progress bar")]
		public Color backgroundColor = Color.white;
		[Tooltip("Color of the filled portion of the progress bar")]
		public Color fillColor = Color.blue;

		[Space]
		[Tooltip("Physical length that the bar occupies")]
		public float length = 0.2f;
		public float size = 0.1f;

		[Tooltip("How far along the progress is currently")]
		[SerializeField] [Range(0, 1)]
		private float progress = 0.5f;

		[Space]
		[Tooltip("Called when the progress changes")]
		public UnityEvent progressChanged;

		/// <summary>How much of the bar is filled</summary>
		public float Progress { get; set; }
	}
	```
* #### SliderElement.cs
	The slider is another UI element used for input. For this element, the input can be anywhere within a range of linear values.

	It consists of two objects, the track and thumb. The thumb is a small sphere that represents the grabbable portion. The track is the line which the thumb can be moved along once grabbed.

	Both of these objects can have their appearance changed using the color properties along with the length and size properties for the track and thumb respectively.

	```cs
	/// <summary>Dispays a bar with a handle that can be relocated to represent some intermediate value</summary>
	[ExecuteInEditMode]
	public class SliderElement : Element {
		[Tooltip("Color of the track the handle is moved across")]
		public Color trackColor = Color.gray;
		[Tooltip("Physical length that the track occupies")]
		public float length = 1;

		[Space]
		[Tooltip("Color of the handle")]
		public Color thumbColor = Color.blue;
		[Tooltip("How large the handle appears")]
		public float size = 0.1f;

		[Tooltip("How far along the handle is across the track")]
		[SerializeField] [Range(0, 1)]
		private float progress = 0.5f;

		[Space]
		[Tooltip("Called when the progress changes")]
		public UnityEvent progressChanged;

		/// <summary>How far along the handle is across the track</summary>
		public float Progress { get; set; }

		/// <summary>Moves the thumb based on the global position</summary>
		/// <param name="position">Global position</param>
		public void Grab(Vector3 position);
	}
	```

## Interactable Scripts:

> Scripts within this category are attached to specific objects that are directly interacted with by the user. To fit other models in the future, different parameters for adjusting the physical locations and dimensions of their components are provided.

> All referenced controller buttons are based on the Oculus Quest 2 controllers.

### Drill.cs

This object is used to create holes at predefined locations on an object in a particular order. The locations and orders are specified using a DrillSequence script on the object to be drilled.

To hold the drill, the hand trigger is held. To drill a hole, the index trigger is pressed.

The drill itself operates by raycasting from the bit position to find a DrillPoint. The DrillSequence associated with this point is then advanced using said DrillPoint.

```cs
/// <summary>Enables drill functionality</summary>
[RequireComponent(typeof(OVRGrabbable))]
public class Drill : MonoBehaviour {
	[Tooltip("Where the drill bit is located on the model")]
	public Vector3 bitPosition;
	[Tooltip("How close the tip needs to be for drilling to occur")]
	public float penetrationDistance = 0.01f;

	/// <summary>Drills a hole at the drill point if possible</summary>
	/// <returns>Drill point where a hole was made</returns>
	public DrillPoint Fire();
}
```

### DrillSequence.cs

This script specifies the means by which an object may be drilled. This is a static approach, so the points and states of the drilled object must be specified.

The steps for making an object drillable goes as follows:

Create an empty GameObject.
Add the DrillSequence script to the object.
Create two empty children of this object: Points and States.

Take the unmodified object that needs to be drilled and add that as a child of the States object.
Drag the object to be drilled into the DrillSequence's states list.
For every hole that needs to be drilled in the object, repeat these 2 steps, substituting the object with a version that has the next hole drilled.

At the end of this, the last object should be what the final result of the drilling process is.

For each hole, create an empty GameObject as a child of the Points object and position them around the corresponding holes.
Add colliders to each of these objects so they match the size of the hole.
Finally, add a DrillPoint script to each of these objects.

Drag all the point objects into the Points field in the DrillSequence script in the order that each hole should be drilled.

```cs
/// <summary>Used to make an object drillable</summary>
public class DrillSequence : MonoBehaviour {
	/// <summary>Defines the order in which points are drilled</summary>
	public DrillPoint[] points;

	/// <summary>State of the object after each point is drilled</summary>
	public GameObject[] states;

	/// <summary>Current step of the drilling sequence</summary>
	public int Step { get; }

	/// <summary>Attempt to advance the sequence to the next step by drilling at the given point</summary>
	/// <param name="point">Point to drill at</param>
	/// <returns>Whether drilling was valid for this point</returns>
	public bool Advance(DrillPoint point);
```

### DrillPoint.cs

This is used by the DrillSequence script to define positions where holes may be drilled.

The best colliders to use for this in practice are sphere colliders.

```cs
/// <summary>A single point on an object that must be drilled</summary>
[RequireComponent(typeof(Collider))]
public class DrillPoint : MonoBehaviour {
	[HideInInspector]
	public DrillSequence sequence;
}
```

### TorqueWrench.cs

The torque wrench is used for tightening bolts.

Despite the currently used torque wrench being a model from Desoutter, its functionality should be applicable to alternative models.

Specifying the head position, head direction, and attachment distance allows the torque wrench to determine how it can be positioned and turned relative to a bolt.

Similar to the drill, the TorqueWrench will raycast in an attempt to find a TorquePoint, which it will then latch on to using the `Attach` method.

Holding the wrench is accomplished by holding the hand trigger and attaching/detaching to a point is toggled through the use of the index trigger.

```cs
/// <summary>Enable torque wrench functionality</summary>
[RequireComponent(typeof(OVRGrabbable))]
public class TorqueWrench : MonoBehaviour {
	[Tooltip("The head of the torque wrench relative to the model")]
	public Vector3 headPosition;
	[Tooltip("The direction in which the head points towards the target (a bolt probably)")]
	public Vector3 headDirection;
	[Tooltip("How close the head needs to be to the target to attach to it")]
	public float attachDistance = 0.01f;

	/// <summary>Current rotation of the wrench about the axis of the target</summary>
	public float Angle { get; } = float.NaN;

	/// <summary>Attach the wrench to an available torque point if possible</summary>
	public TorquePoint Attach();

	/// <summary>Detach the wrench from the torque point it is currently attached to</summary>
	public void Detach();
```

### TorquePoint.cs

This is used by the TorqueWrench script to define positions where attachement may occur.

These torque points also carry information regarding the tightness of the object, which is probably a bolt. The tightness value is calculated as the degrees the wrench turned while attached multiplied by the angle ratio in the direction of angle sign.

In practice, the best colliders to use for this are sphere colliders.

```cs
// <summary>Where the head of the torque wrench snaps to</summary>
public class TorquePoint : MonoBehaviour {
	[Tooltip("Normal direction on the surface opposite to the direction that the target should be screwed in at")]
	public Vector3 surfaceNormal = Vector3.forward;

	[Tooltip("How far back the target is moved")]
	public float tightness;
	[Tooltip("Maximum tightness the target can achieve")]
	public float maxTightness = Mathf.Infinity;

	[Tooltip("Ratio at which tightening angle is converted into tightness")]
	public float angleRatio = 1f / 360f;
	[Tooltip("Angular direction (clockwise or counterclockwise) that increases tightness; this value should be either 1 or -1")]
	public float angleSign = 1;

	[Space]
	[Tooltip("Object being tightened into a given direction")]
	public Transform target;
	[Tooltip("Direction to tighten the target into")]
	public Vector3 targetDirection;

	/// <summary>Turns the target, increasing tightness</summary>
	/// <param name="angle">Angle in degrees to tighten the point</param>
	public void Turn(float angle);
}
```

### NailGun.cs

The nail gun utilizes Unity's rigidbody physics and joints system to pin objects to others.

Similar to the other tools, the nail gun has paramters to adjust its nozzle locatio and uses a raycast to detect objects to pin. Unlike the other tools, the nail will raycast through multiple objects according to its detection distance.

Objects that have raycast through will be affected differently based on their components. Immovable objects (those without rigidbodies or kinematic rigidbodies) will act as a surface to which movable objects are pinned. Objects with rigidbodies are jointed to this surface, preventing them from moving or rotating independtly. This method does not require the pinned objects to be childed to the object they are pinned to.

```cs
/// <summary>Enables nail gun functionality</summary>
[RequireComponent(typeof(OVRGrabbable))]
public class NailGun : MonoBehaviour {
	[Tooltip("Nail object that the gun dispenses")]
	public GameObject nailPrefab;
	[Tooltip("Where the nozzle of the nail gun is located on the model")]
	public Vector3 nozzlePosition;
	[Tooltip("Offset from the nozzle where the nail should be spawned")]
	public Vector3 nailOffset;
	[Tooltip("Rotation of the nail after being dispensed")]
	public Vector3 nailRotation;

	[Tooltip("Distance out from the nozzle to detect objects to pin together")]
	public float detectionDistance = 0.01f;

	public RaycastHit[] Fire();
}
```

### StationDoor.cs

This script is used by all the tooling station containers to open their door.

It has parameters for the speed at which the door opens along with the colors representing the task status.

Currently, the kiosk-like stands to the sides of the tooling stations are used to activate this script through a built-in window.

```cs
/// <summary>Enables tool station doors to be opened</summary>
public class StationDoor : MonoBehaviour {
	[Tooltip("Speed at which the door opens")]
	public float speed = 0.05f;

	public new GameObject light;
	public Material redLightMat, yellowLightMat, greenLightMat;

	/// <summary>Current progress of the station</summary>
	public Completion Progress { get; set; }

	public void SetInactive();
	public void SetIncomplete();
	public void SetComplete();
}
```

### Tablet.cs

The tablet object is used contain windows within itself. It can be simultaneously held by the user and interacted with at once.

The window which it opens may be specified using its `windowPath` property.

```cs
/// <summary>
/// Enables tablet functionality
/// 
/// The tablet can be held while displaying a window that can be interacted with while being held
/// </summary>
[RequireComponent(typeof(Rigidbody))]
[RequireComponent(typeof(Interactor))]
public class Tablet : MonoBehaviour {
	public string windowPath;
	public Vector3 windowOffset;
}
```

## Interaction Scripts:

> Scripts within this category are used as the backbone behind interaction in the interactable objects.

### InterfaceUsage.cs

This script is bound to the OVRCameraRig instance within the scene.

It is used to enable clicking and grabbing on UI elements as well as a means of creating rulers and spawning the tablet.

It detects these interactions using the pointers placed in the scene for each hand.

```cs
/// <summary>Enables user interaction with Display content on the VRUI layer</summary>
public class InterfaceUsage : MonoBehaviour {
	public static InterfaceUsage Instance => Object.FindObjectOfType<InterfaceUsage>();

	[Header("Pointers")]
	public Pointer left;
	public Pointer right;

	[Header("Buttons")]
	public OVRInput.RawButton leftClickButton, leftGrabButton;

	[Space]
	public OVRInput.RawButton rightClickButton, rightGrabButton;

	[Space]
	public OVRInput.RawButton menuButton;
	public OVRInput.RawButton rulerCreateButton, rulerClearButton, rulerDirectButton;

	[Header("Options")]
	public float menuSpawnDistance = 1;
	public float rulerAdjustSpeed = 1;
	public Ruler.UnitSystem rulerUnitSystem;
	public bool rulerDoubleSided;

	[Header("Prefabs")]
	public GameObject tabletPrefab;
}
```

### Pointer.cs

The pointer is the means by which interaction with the UI system is detected.

There should always be two pointers in each scene, bound to the correct controller and parented to the OVRCameraRig.

```cs
/// <summary>Used to reperesent hand location and objects of interest for VRUI Display content</summary>
[RequireComponent(typeof(LineRenderer))]
[RequireComponent(typeof(Collider))]
public class Pointer : MonoBehaviour {
	[Tooltip("Max distance that the pointer can select objects")]
	public float length = 1f;

	[Tooltip("Color to use for the pointer line when it hits an object")]
	public Color hitColor = Color.blue;

	[Tooltip("Color to use for pointer line when it hits an interactable object")]
	public Color interactorColor = Color.yellow;

	[Space]
	[Tooltip("Controller that the pointer is associated with")]
	public OVRInput.Controller controller = OVRInput.Controller.All;

	/// <summary>Whether the pointer is currently being used</summary>
	public bool Active => (OVRInput.GetActiveController() & controller) == controller;

	/// <summary>Hand for the controller</summary>
	public Touch.Hand Hand => Touch.GetHand(controller);

	/// <summary>Start point of the pointer</summary>
	public Vector3 StartPoint => transform.position;

	/// <summary>End point of the pointer</summary>
	public Vector3 EndPoint { get; }

	/// <summary>Whether the pointer should select from a distance</summary>
	public bool ShouldRaycast { get; set; }
}
```

### Interactor.cs

By assigning an object an this script, it becomes visible to UI interactions.

All the fields of this class are events that occur based on how the user interacts with the objects the components are placed on.

```cs
/// <summary>Enables an object to begin some behaviour or perform some action when interacted with by pointers</summary>
public class Interactor : MonoBehaviour {
	/// <summary>Called when while a pointer hovers over the collider</summary>
	public Action<Pointer> hover, hoverEnter, hoverExit;

	/// <summary>Called when a controller/hand comes into contact with the collider</summary>
	public Action<Pointer> contact, contactEnter, contactExit;
}
```
