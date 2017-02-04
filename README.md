# Notes-Inbox
Inbox for notes.

## Table of Contents

* [(S)CSS](#scss)
* [Unity](#unity)
* [Pandoc](#pandoc)
* [Google Maps and Street View API](#google-maps--street-view-api)
* [Terminal](#terminal)
* [Jupyter](#jupyter)

## (S)CSS

### Resources

* [Dropbox (S)CSS Style Guide](https://github.com/dropbox/css-style-guide)

## Unity

### References

* [C# Tutorials](http://catlikecoding.com/unity/tutorials/)
* [How to Submit to App Store](https://unity3d.com/learn/tutorials/modules/beginner/platform-specific/how-to-submit-to-the-ios-app-store)
* [NPR sketch shader vvvv](http://www.thomaseichhorn.de/npr-sketch-shader-vvvv/) by Thomas Eichhorn.

### Scripts

* [Singleton.cs](https://github.com/nonoesp/Notes-Inbox/blob/master/Unity/Scripts/Singleton.cs)

### Normalize Mouse Screen Position

```csharp
// Normalize Mouse Screen Coordinates to [-1, 1]
Vector2 NMouse = new Vector2 (2 * Input.mousePosition.x / Screen.width - 1, 2 * Input.mousePosition.y / Screen.height - 1);
```

### Raycast Object to Check Mouse Over

Inside the Update() method of an object's script, add the following.

```csharp
RaycastHit hit;
Ray ray = Camera.main.ScreenPointToRay (Input.mousePosition);
bool wasHit = Physics.Raycast (ray, out hit);

if (wasHit) {
	Debug.Log ("Sphere was hit.");

} else {
	Debug.Log ("Sphere wasn't hit.");			
}
```

### Change Material Properties

```csharp
// Color
this.GetComponent<MeshRenderer>().material.color = Color.white;
// Emission color
this.GetComponent<Mesh<Renderer>().material.SetColor ("_EmissionColor", Color.black);
```

### Format a Float to Decimal

```csharp
String.format("%.2f", floatValue);
```

### Avoid Rigid Bodies to Pass Through a Custom Mesh Terrain Object

This function assumes you have a GameObject tagged `Terrain` which is the base terrain of your game with a MeshCollider and a kinematic Rigidbody. (Be sure to add the tag, MeshCollider and Rigidbody to the mesh object and not to its wrapper.)

```csharp
	void Update() {
		// Update code
		this.ClampHeightToTerrain();
	}

	void ClampHeightToTerrain() {
		GameObject terrain = GameObject.FindGameObjectWithTag ("Terrain");
		if (terrain != null) {
			Ray r = new Ray (transform.position + Vector3.up * 20f, Vector3.down);
			RaycastHit hit;
			terrain.GetComponent<MeshCollider> ().Raycast (r, out hit, 100f);
			Collider col = GetComponent<Collider> ();
			Vector3 p = transform.position;
			// todo: add collider half height to clamp minimum
			float y = Mathf.Clamp(p.y, hit.point.y + col.transform.localScale.y * 0.5f, 30.0f);
			transform.position = new Vector3 (p.x, y, p.z);
		}
	}
```

### Create Manual Boundaries from Mesh and Clamp GameObjects

```csharp
	void Update() {
		// Update code
		this.ClampBoundaries();
	}

	void ClampBoundaries() {
		GameObject[] boundaries = GameObject.FindGameObjectsWithTag ("Boundary");
		foreach (GameObject boundary in boundaries) {
			if (boundary != null) {

				Ray r;
				RaycastHit hit;
				Vector3 p = transform.position;
				MeshCollider meshCollider = boundary.GetComponent<MeshCollider> ();
				float x = p.x;
				float z = p.z;
				float clampOffset = 1.0f;

				// Z

				// z-forward
				r = new Ray (transform.position, Vector3.forward);
				meshCollider.Raycast (r, out hit, 100f);

				if (p.z <= hit.point.z && hit.point.magnitude != 0.0) {
					if (p.z > hit.point.z - clampOffset) {
						z = hit.point.z - clampOffset;
					}
				}

				// z-back
				r = new Ray (transform.position, Vector3.back);
				hit = new RaycastHit();
				meshCollider.Raycast (r, out hit, 100f);

				if (p.z > hit.point.z && hit.point.magnitude != 0.0) {
					if (p.z < hit.point.z + clampOffset) {
						z = hit.point.z + clampOffset;
					}
				}

				// X

				// x-forward
				r = new Ray (transform.position, Vector3.right);
				hit = new RaycastHit();
				meshCollider.Raycast (r, out hit, 100f);

				if (p.x <= hit.point.x && hit.point.magnitude != 0.0) {
					if (p.x > hit.point.x - clampOffset) {
						x = hit.point.x - clampOffset;
					}
				}

				// x-back
				r = new Ray (transform.position, Vector3.left);
				hit = new RaycastHit();
				meshCollider.Raycast (r, out hit, 100f);

				if (p.x > hit.point.x && hit.point.magnitude != 0.0) {
					if (p.x < hit.point.x + clampOffset) {
						x = hit.point.x + clampOffset;
					}
				}

				transform.position = new Vector3 (x, p.y, z);

			}
		}
	}
```

### Load a Bytes File as A Resource

(Source: http://answers.unity3d.com/questions/1025651/loading-bytes-resource-as-textasset-to-get-byte.html)

```csharp
// Option A
Resources.Load<TextAsset>("path");
// Option B
Resources.Load("path", typeof(TextAsset));
```

### Get File Paths

```
// Universal
string[] filePaths = Directory.GetFiles (Application.streamingAssetsPath + "/" + folderName + "/", "*.bytes", SearchOption.AllDirectories);
// iOS (the universal form works also on iOS)
string[] filePaths = Directory.GetFiles (Application.dataPath + "/Raw" + "/" + folderName + "/", "*.bytes", SearchOption.AllDirectories);
string[] filePaths = Directory.GetFiles (Application.dataPath + "/Raw", "*.bytes", SearchOption.AllDirectories);
```

### Project Point to a Mesh

This script supposes that you have a mesh tagged `Terrain` in your Unity scene.

```
public Vector3 ProjectToTerrain(Vector3 p) {
		GameObject terrain = GameObject.FindGameObjectWithTag ("Terrain");
		if (terrain != null) {
			Ray r = new Ray (p + Vector3.up * 20f, Vector3.down);
			RaycastHit hit;
			terrain.GetComponent<MeshCollider> ().Raycast (r, out hit, 100f);
			Collider col = GetComponent<Collider> ();
			return hit.point;
		}
		return new Vector3();
	}
```

## Pandoc

### Convert to ICML (InDesign)

If the -s option isn't used, which means standalone, the ICML file won't usually work on InDesign.

```
pandoc -s Text.md -o Text.icml
```

### Latex File

As you have discovered, `--include-in-header` adds text into the
preamble specified in Pandoc's LaTeX template. There are a few ways to do what you are trying to do.

(1) If you would like a completely custom preamble, you need to specify a template file using

	pandoc -o output.tex --template=FILE input.txt

The template can have variables (such as `$title$` and, more importantly, `$body$`) and conditionals. If you would like some inspiration, you could check out the default template using the command:

	pandoc -D latex

(2) If you want to use a new template once and for all, you can make one, call it `default.latex`, and put it in the templates directory (`~/.pandoc/templates/` on a unix machine). In this case, you need to specify that you want to use a template by calling:

	pandoc -o output.tex --standalone input.txt

(3) If you would rather not deal with templates at all, you can just run:

	pandoc -o output.tex input.txt

and the result will be a bare LaTeX document, that is, without a preamble, `\begin{document}` or `\end{document}`. Then you can add a preamble yourself. Note that any metadata (title, author) will be lost when using this method.

Full details on how to make and use templates can be found in [Pandoc's excellent man page](http://johnmacfarlane.net/pandoc/README.html).

(Source: [SuperUser](http://superuser.com/questions/356032/markdown-to-latex-conversion-with-a-custom-preamble-using-pandoc))

### Running biber

Seems like you din’t run `bibtex` but only `(pdf)latex`, right?

LaTeX cares for typesetting the bib but you need another program
(`bibtex`) to preprocess your `.bib` to be used by `pdflatex`. That means that you’ll need the following steps to get a bibliography

1.  Create you `tex` file and the `bib` file.
2.  Add the bib to your document with `\bibliography`
3.  Run `pdflatex mydoc.tex` to get a first version of your document knowing which entries of your bib are needed
4.  Run `bibtex mydoc` to let `bibtex` do the preprocessing based on the `aux` file. That’ll generate `mydoc.bbl` including all information that `pdflatex` needs
5.  Run `pdflatex mydoc.tex` again to get the new `bbl` file included in your document

For further information please consult any (good) book about LaTeX this is a basic topic and should be explained there.

-----

Please note that `bibtex` is kind of outdated. The better way is to use `biber` in combination with the package `biblatex`, which provides a great tool to format your citations and bibliography.

(Source: [StackExchange](http://tex.stackexchange.com/a/85150))

## Google Maps & Street View API

### Reference

* [Google Maps JavaScript API V3 Reference](https://developers.google.com/maps/documentation/javascript/reference)
* [Custom controls](https://developers.google.com/maps/documentation/javascript/examples/control-custom)
* [Simple styled maps](https://developers.google.com/maps/documentation/javascript/examples/maptype-styled-simple)
* [Complex styled maps](https://developers.google.com/maps/documentation/javascript/examples/maptype-styled-complex)

### Set Map's Center

```javascript
map.setCenter(location);
```

### Pan to a Location

```javascript
map.panTo(location, duration);
```

### Place a Marker

```javascript
var marker = new google.maps.Marker({
  position: latLng,
  map: map
});
```

## Terminal

### Rename a File

```
mv oldname newname
```

### Output a Date

```
$(date) // simple date
$(date + %Y) // date with format, just outputs a year
$(date + %Y-%M-%D) // date with format, outputs yyyy-mm-dd
```

### Add ssh Key

```bash
ssh-add -K [path/to/private SSH key] # e.g. ~/ssh/id_rsa
```

## Jupyter

### Autoload Imported Files Each Time Your Run Any Piece of Code

Just add this to the top of your Jupyter notebook and run it once.

```
%load_ext autoreload
%autoreload 2
```

## Me

I'm [Nono Martínez Alonso](http://nono.ma) ([nono.ma](http://nono.ma)), a computational designer with a penchant for design, code, and simplicity.  
I tweet at [@nonoesp](http://www.twitter.com/nonoesp) and write at [Getting Simple](http://gettingsimple.com/). If you find this useful, I would love to hear about it. Thanks!
