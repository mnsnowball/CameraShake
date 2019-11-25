# Camera Shake System Tutorial
This tutorial will cover the creation of a camera shaking system within Unity. It is fairly simple.

### Setting up the scene
Create a new scene in Unity.

Create an empty GameObject in the scene. Call it "CameraHolder". Reset its transform and child the MainCamera onto it.

Create two scripts, one called "CameraShake" and one called "TriggerShake". Drag the CameraShake script onto the MainCamera
(not the CameraHolder object)

Create another empty GameObject, call it TriggerShakeObj. Add the TriggerShake script to this object.

Create a few objects in the scene and turn the camera via the cameraHolder object so that it is within view of them. Create a new colored
material if you'd like and assign it to one of the objects to make them stand out. This step, while unnecessary, will help
to make the effects of the camera shaking more obvious.

Create a button on the canvas and change its text to say "Shake!"

##### The CameraShake script

Add the following function in the CameraShake script:

```C#
public IEnumerator Shake(float duration, float magnitude)
    {
        Vector3 originalPos = transform.localPosition;
        float elapsed = 0.0f; // keeps track of time since started shaking

        while (elapsed < duration)
        {
            float x = Random.Range(-1f, 1f) * magnitude;
            float y = Random.Range(-1f, 1f) * magnitude;

            transform.localPosition = new Vector3(x, y, originalPos.z);

            elapsed += Time.deltaTime;

            yield return null;
        }

        transform.localPosition = originalPos;
    }
```

This IEnumerator takes a float for the duration and magnitude of the shaking. It gets the original position to revert back to after
the shaking is done and uses a while loop to shake the camera. Each frame the camera will be set to a new position in 
the x and y direction that is multiplied by the camera magnitude. It adds the time that has elapsed since the last frame to accurately
reflect the timing and returns. It runs until the IEnumerator finishes.When the camera is done shaking, it resets back to its original
position and finishes the shake.

##### The TriggerShake script

Add the following code to the TriggerShake script:

```C#
public CameraShake shaker;

public void ShakeCamera() {
    StartCoroutine(shaker.Shake(0.15f, 1.0f));
}
```

This code simply provides a way to access the CameraShake script via the shaker variable and provides a way to invoke the camera
shake with a public function.


##### Putting it all together

Click on the canvas button. In the OnClick section in the inspector, click on the plus button to add a new behaviour and drag in the
ShakeTriggerObject. In the drop-down list, click on TriggerShake > ShakeCamera.


Click on the TriggerShakeObject. In the TriggerShake script component, drag the MainCamera into the space for the Shaker.

Now when you click on the button, it should shake the camera in a noticeable way.

Enjoy!
