# s21-boeing

Technical Documentation

## Main Menu Scripts:

### ChangeScene.cs

This script is used on the scene changing buttons of the Main Menu’s tablet to load the user into their selected scene. If the user hits the “Main Menu” button, the function “ChangeToMenu()” is called to send them to the main menu. If the user hits the “SandboxTutorial” button, the function “ChangeToSandbox()” is called to send them to the sandbox scene. Lastly, if the user hits the “Tooling Practice” button, the function “ChangeToTooling()” is called to send the user to the ToolingEnvo scene.
 
## Sandbox Tutorial Scripts:

### TaskCycle.cs

TaskCycle.cs is the script that handles the task rotation accessed by the “Task” button of the SandboxTutorial’s tablet. This window shows the user every task available to them, allows them to cycle to each task, provides a complete/incomplete button, and provides a “Back” button to return to the menu. The script only handles which task is shown to the user and the buttons that transition between tasks.
To start, the script initializes the task number to “1”, activates task number 1, and deactivates the rest of the tasks.
 
Upon the user hitting either the “back” button or the “next” button, the script will then decrement the task number by 1 for hitting back, or increment the task number by 1 for hitting next. If the value of task number goes below 1, task number will be manually set to 6, the last task page, for cycling purposes. The same goes for the task number going above 6, the task number will be set to 1, the first task page, for cycling purposes. Each time the user hits back or next, the script will first deactivate the currently shown task window and then activate the new task window along with any buttons necessary for its functionality.


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
