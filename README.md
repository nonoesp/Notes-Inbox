# Notes-Inbox
Inbox for notes.

## Table of Contents

* [(S)CSS](#scss)
* [Unity](#unity)
* [Pandoc](#pandoc)
* [Google Maps and Street View API](#google-maps--street-view-api)

## (S)CSS

### Resources

* [Dropbox (S)CSS Style Guide](https://github.com/dropbox/css-style-guide)

## Unity

### References

* [Unity C# Tutorials](http://catlikecoding.com/unity/tutorials/)

### Raycast Object to Check Mouse Over

Inside the Update() method of an object's script, add the following.

```
RaycastHit hit;
Ray ray = Camera.main.ScreenPointToRay (Input.mousePosition);
bool wasHit = Physics.Raycast (ray, out hit);

if (wasHit) {
	Debug.Log ("Sphere was hit.");

} else {
	Debug.Log ("Sphere wasn't hit.");			
}
```

## Pandoc

### Convert to ICML (InDesign)

If the -s option isn't used, which means standalone, the ICML file won't usually work on InDesign.

```
pandoc -s Text.md -o Text.icml
```

## Google Maps & Street View API

### Reference

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

### About Me

Hi there! My name is Nono Martínez Alonso ([@nonoesp](http://twitter.com/nonoesp)) ([www.nono.ma](http://nono.ma)).  
I write at [Getting Simple](http://gettingsimple.com) and [Nono Says](http://nono.ma/says).  
I also do [Getting Architecture Done](http://gettingarchitecturedone.com) and [Viewtee](http://viewtee.com).
