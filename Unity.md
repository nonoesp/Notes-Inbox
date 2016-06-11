_Notes Unity

## References

* [Unity Scripting](http://docs.unity3d.com/Manual/ScriptingSection.html)
* [BackgroundPlane.cs](https://gist.github.com/pyrobot/4363640)
* [Attaching and Removing a script to the main camera](http://answers.unity3d.com/questions/255799/attaching-and-removing-a-script-to-the-main-camera.html)
* [Unity - Coding in Unity for the Absolute Beginner](https://unity3d.com/learn/tutorials/modules/beginner/live-training-archive/coding-for-the-absolute-beginner)
* [Procedurally Generated Content](http://www.xataka.com/videojuegos/procedurally-generated-content-la-revolucion-de-los-videojuegos-es-ahora-aunque-llevamos-40-anos-creandola?utm_source=twitter.com&utm_medium=social&utm_campaign=buffer&utm_content=buffer71420)

## 

* Frame-rate independence (speed * Time.deltaTime)
* cmd + opt + c

advocate


### Logging

```
Debug.Log("I am alive!");
```

### Keyboard Input

Edit > Project Settings > Input

* Scripting: Input.GetAxis(“_NAME_”);
* GetKeyDown / GetKey / GetKeyUp
* Mapping vectors to keycodes.


### Good Practices

* Have a Rhino layer for each material.
* Texture mapping (BoundingBox).
* make objects static (bake lightmap)
* spotlight baking to mix


### Learning from Unity

### Colliding Objects (Raycasting to a single object)

http://docs.unity3d.com/ScriptReference/Collider.Raycast.html

### Shaders

* Properties are what the artist can edit inside the material editor in Unity.

## Creating a Singleton Class

* Create a new C# Script.
* Remove both Start() and Update() methods.
* Add the following:

```
public class Singleton<T> : MonoBehavior Where T : MonoBehavior {


}
```

{T} is a placeholder for the class, where we let the class know that the class will be defined later.

