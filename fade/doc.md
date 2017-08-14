# Fade effect

The "fade effect" is a transitionary visual effect applied to objects when certain events occur. Six event types have been identified, each in various states of implementation both in the application and when edited (see paragraphs below for editing the effect).

| Event | Description | Application status | Edition status |
|-----  |-----        |-----               |-----           |
| *Elements enter/leave domain* | This event should occur when entities are loaded for the first time and are ready to be displayed after the user enters a new domain | The application applies this fade event only to model entities. | Edition is possible on model, avatars, polyvoxel and polyline entities |
| *Bubble intersection - Owner POV* | This event should occur when a user with his bubble activated has his avatar's bubble intersect another avatar | The application doesn't manage this event | Edition is possible on model, avatars, polyvoxel and polyline entities |
| *Bubble intersection - Trespasser POV* | This event should occur when a user's avatar trespasses on another user's avatar which has an enabled bubble | The application doesn't manage this event | Edition is possible on model, avatars, polyvoxel and polyline entities |
| *Another user leaves/arrives* | This event should occur when a user's avatar enters or leaves a domain | The application launches this domain normally | Edition is possible on model, avatars, polyvoxel and polyline entities |
| *Changing an avatar* | This event should occur when a user changes the appearance of his avatar | The application doesn't manage this event | Edition isn't supported and is ignored |

## Description

### Gradients
The fade effect is based on the thresholding of a gradient in 3D space. The gradient has values between 0 and 1. When the gradient has a value beneath a user defined threshold, the object is transparent. This gradient is the sum of a base radial gradient centered on the object's position (with value 0 at this position) and a noise gradient. Each base and noise gradient level can be modified as well as the size of these gradients in the three dimensions. The center of the base gradient is furthermore offset in position depending on the type of fade event.
![2D gradient equivalent](/fade/gradients.jpg "2D equivalent (from left to right) : base gradient (here a linear ramp), noise gradient and resulting mix of the two with threshold at 0.4")
The threshold result can be inverted with the "Invert" check box in which case the transparent pixels correspond to gradient values above the threshold.

### Edges
The outer edge of the fade effect is the limit between transparent and opaque pixels. The inner edge is set with the "edge width" parameter, between 0 and 1 as a ratio of the gradient size. Both edges have separate colors and color intensities, in the emissive channel.

### Noise
The 3D noise is created semi-procedurally. While it is based on an image (*interface/resources/images/fadeMask.png*), this image, at this time, is only used to seed the procedural noise. For this reason, the image has an influence on the overall look of the noise (for instance a smoothly varying *fadeMask.png* will give a rounder looking noise) but it doesn't ncessarily imply a direct visual correspondance in the 3D noise. At this time only the red channel of the *fadeMask.png* image is used in the output noise.

## Fade editing
The *debugFade.js* script in *scripts/developer/utilities/render* can be used to tweak the fade effect on the 4 supported events, the above two being the only ones processed in the application at this time. Checking the "Edit Fade" checkbox applies cyclicly the selected fade event to the object in front of you. To manually control the threshold value, click on "Manual" under "Edit Fade". In this mode, the "Threshold" slider controls the value.
All changes to the various fade parameters apply to the currently selected event.

### Parameters
Here is the list of the paramters of the fade effect from top to bottom and left to right in the edit window. For the *Another user leaves/arrives* event, the **base gradient size** is driven automatically by the application and is set to the objects bounding box dimensions. When this event is selected, changing the base gradient size in the edit window will be ignored.

| Name                                   | Description                                                                       | Examples |
|----------                              |--------                                                                           |-------   |
| Edit Fade (checkbox and dropdown list) | Applies a fade event of the type selected in the dropdown list to a picked object | ![Element enter event](/fade/Fade-EditOn.jpg "Element enter/leave event applied")![Avatar enter event](/fade/Fade-EditAvatar.jpg "Avatar enter/leave event applied") |
| Manual (checkbox)                      | When enabled, enables to manually sets the threshold value                        |           |
| Threshold (slider)                     | The threshold value in *manual* mode                                              | ![Manual threshold](/fade/Fade-Manual50.jpg "Manual threshold at 50%")         |
| Invert (checkbox)                      | Inverts the thresholding behavior                                                 | ![Inverted](/fade/Fade-Inverted.jpg "Manual threshold at 50% with invert enabled")         |
| Base Gradient                          | Parameters of the base radial gradient                                            |          |
| > Size X, Y and Z (sliders)            | Controls the scale of the radial gradient in all three dimensions                 | ![Base size](/fade/Fade-BaseSize.jpg "Base gradient size stretched in Y & Z with a small X size") ![Base size bigger](/fade/Fade-Bigger.jpg "Base gradient size stretched in Y & Z and with a medium X size")         |
| > Level (slider)                       | The level of contribution of the base radial gradient to the final gradient       | ![Base level](/fade/FadeBiggerNoiser.jpg "Base gradient at half level, making the noise stand out more")         |
| Noise Gradient                         | Parameters of the 3D noise gradient                                               |          |
| > Size X, Y and Z (sliders)            | Controls the scale of the noise gradient in all three dimensions                  | ![Noise size](/fade/Fade-NoiseSize.jpg "Noise with different size values in X,Y and Z")         |
| > Level (slider)                       | The level of contribution of the noise gradient to the final gradient             | ![Noise level](/fade/Fade-NoNoise.jpg "Noise level at 0 makes a for clear noiseless clip if the base level is at 1") ![Noise level](/fade/Fade-BaseAndNoise.jpg "Noise level at full 1 value adds noise to the clip edge")        |
| Edge                                   | Parameters of threshold edge                                                      |          |
| > Width (slider)                       | Width of the edge in gradient value unit                                          | ![Edge width](/fade/Fade-SmallEdgeWidth.jpg "Very small edge width") ![Edge width](/fade/Fade-MedEdgeWidth.jpg "Medium edge width")         |
| > Inner color                          | Color and intensity parameters of the edge on the opaque side of the object       |           |
| >> Color red, green and blue (sliders) | RGB color of the edge on the opaque side                                          | ![Inner edge color](/fade/Fade-InnerRed.jpg "Setting the edge inner color to pure red")        |
| >> Color intensity (slider)            | Intensity of the color. This value can be greater than 1                          | ![Inner edge level](/fade/Fade-Inner0.jpg "Setting the edge inner level to 0")          |
| > Outer color                          | Color and intensity parameters of the edge on the transparent side of the object  |          |
| >> Color red, green and blue (sliders) | RGB color of the edge on the opaque side                                          | ![Outer edge color](/fade/Fade-OuterYellow.jpg "Setting the edge outer color to pure yellow")        |
| >> Color intensity (slider)            | Intensity of the color. This value can be greater than 1                          | ![Outer edge level](/fade/Fade-Outer0.jpg "Setting the edge outer level to 0")          |
| Timing                                 | Parameters related to the animated elements of the fade events                    |          |
| > Duration (slider)                    | The duration in seconds of an event (this only applies to both enter / leave events)                                          |          |
| > Curve (dropdown list)                | The timing curve applied to the threhold value                                    |          |
| > Noise animation                      | Parameters linked to the automatic noise gradient animation                       |          |
| >> Speed X, Y, Z                       | Translation speed of the noise gradient in all 3 directions                       |          |

### Save
Clicking on the save button saves a JSON configuration file under interface/resources/config with name corresponding to the currently edited event. This file contains the current settings only for the edited event.
Warning : all saves will overwrite the same file so if you wish to keep multiple settings, you must copy the files to another location.
### Load
Clicking on the load button loads back the settings stored in the JSON configuration file under interface/resources/config with name corresponding to the currently edited event.

**WARNING**: the JSON configuration files are not automatically loaded at launch time.
