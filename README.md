# Launch Control: Touchdesigner on Steroids :fire:

This repo expands (x4-x8) the functionality of the Novation Launch Control Midi Controller by utilizing `Python programming`, `LED programming` and `TD mappings`. This is an effort to re-use cheap old/limited gear while avoiding buy new MIDI controllers for 500$++ (for fuck shake!)

The motivation behind this development was to overcome the limitation of the Launch Control's 8 + 4 buttons in User Mode for visual shows and performances. As my performances often require more than 20 scenes in TouchDesigner, this software bypasses the LC circuit limitation and provides support for up to `32 scenes`, and I am working to further boost it up to total of `64 scenes`.

On the example bellow you can see the Group Buttons lighting in Red (in this case the first group), and the Scene Buttons lighting in green (in this case the first scene)


![Alt text](images/LaunchControl.jpg?raw=true "Title")





----
## Usage:

`launch_control_template.toe` (use my routing)

In case you want to structure your project in the above format (8 buttons x 4 groups), there is this ready-to-use Touchdesigner template file in order to achieve that. Everything works (lights, mappings, cirquits) in the way described. Open my template project, and build yours on top. You have 32 nulls, as the endpoints of your 32 scenes. 

A smart idea would be (for cpu/gpu optimization) to run your global effects after the `switch`, and not to every scene or group seperetaly (this will reduce cooking). Another important detail is to close the viewer mode of every component (top left), so only the selected components cook. If you do it properly, you can in theory run unlimited amount of scenes.



`mappings.tox` (run a custom routing)



In case you just need the midi mappings for the Launch Control (i know its tedious work), you can use mine. There are all the sliders starting from the left side (s1,s2,s3,s4,s5,s6,s7,s8,s9,s10,s11,s12,s13,s14,s15,s16) pre-mapped, as well as all the buttons start from the left side (b1,b2,b3,b4,b5,b6,b7,b8,) and the group buttons (b9,b10,b11,b12). You can use that in order to run a custom midi configuration. 

Open the mappings.tox in Touchdesigner locate the midi component (midi) and copy and paste it to your corresponding midi component local/midi on your project (delete yours, add mine). You may need to add the device under Dialogs/Midi Device Mapper, then the midiinmap component should see it.

----
## Logic

The logic resides on a `switch` Touchdesigner cirquit ranging from values (0-31). These values are divided in 4 groups by the 4 buttons on the right side of the midi controller, expanding the functionality of the 8 frontal midi buttons x4. Additionally every button you hit on the controller returns a visual feedback, in order for the user to perceive the current state. These scripts/mappings currently work for User mode (top 2 buttons in Launch control), and I am developing the Factory mode as well for a complete x8 controller expansion. (8 buttons x8 functionality)

![Alt text](images/switch.png?raw=true "Title")


In terms of programming, for the (b1,b2,b3,b4,b5,b6,b7,b8) I use the following routing. In case a user selects a bank we just force the index of the ```switch``` scene to start from the scene number of the selected bank. For example if the user wants to select Scene 4, Bank 3 the index of the ```switch``` will be:

    switch1 idx : 8 * (3-1) + 4

Or analytically:

    switch1 idx : (numOfScenes) * (selBank - 1) + (selScene)



## Scripts

There are 3 python scripts incorporated inside the project:

- ```chopexec1```: controls the Banks
- ```chopexec2```: controls the Scenes
- ```chopexec3```: controls the LEDS


You can run your custom configuration by changing these chops, on-top of mine. An example of the Bank Selection script would be:

```python
def onValueChange(channel, sampleIndex, val, prev):

	if op('constant2')[0] > 0:

		if op('constant2')[4] > 0:
			op('switch1').par.index = 0
		elif op('constant2')[5] > 0:
			op('switch1').par.index = 1

    # goes on for 2 - 31 . . .
```




## Contributions - TODO

If you like the idea of not paying irrational ammount of money for the same rebranded digital crap from 1970, you can contribute to this repo on the following matters (or brainstorm new ones) :

1) Expand the functionality for the Factory Page as well, and boost the LC on super-steroids. (64 scenes) -- (Have to examine this carefully cause Factory Button forwards ```sysex``` messages)

2) Expand the functionality of the sliders as well. Currently they are global (16 sliders), the idea is to go per Bank (16 x4 or x8)

3) Incorporate LEDs everywhere.

4) Longshot: Can try encoding banks in a smarter way, for example incorporate multiple button presses (B1+B3 = B5), that way the controller can be endless.


## Author Notes: 

Star/ Fork/ Mention in your project :fire:
