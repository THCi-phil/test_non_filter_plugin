This is an edited version of https://github.com/imagej/example-legacy-plugin/

With corrections to the instructions so they work !


This is an example Maven project implementing an ImageJ 1.x plugin.

For an example Maven project implementing an **ImageJ2 command**, see:
    https://github.com/imagej/example-imagej2-command

It is intended as an ideal starting point to develop new ImageJ 1.x plugins
in an IDE of your choice. You can even collaborate with developers using a
different IDE than you.

Since this project is intended as a starting point for your own
developments, it is in the public domain.

How to use this project as a starting point
===========================================

1. Visit [this link](https://github.com/imagej/example-legacy-plugin/generate)
   to create a new repository in your space using this one as a template.

2. [Clone your new repository](https://help.github.com/en/articles/cloning-a-repository).

3. Edit the `pom.xml` file. Every entry should be pretty self-explanatory.
   In particular, change
    1. the *artifactId* (**NOTE**: should contain a '_' character)  ***EDIT MUST contain***
    2. the *groupId*, ideally to a reverse domain name your organization owns
    3. the *version* (note that you typically want to use a version number
       ending in *-SNAPSHOT* to mark it as a work in progress rather than a
       final version)
    4. the *dependencies* (read how to specify the correct
       *groupId/artifactId/version* triplet
       ***EDIT broken link, searching on imagej.net.Maven for how to find a dependency turns up useful information, but not "the correct" triplet ***
       [here](https://imagej.net/Maven#How_to_find_a_dependency.27s_groupId.2FartifactId.2Fversion_.28GAV.29.3F))
    5. the *developer* information
    6. the *scm* information

4. ***Edit: actually more reliable to do this after, get the basic thing working first***
  Remove the `Process_Pixels.java` file and add your own `.java` files
   to `src/main/java/<package>/` (if you need supporting files -- like icons
   -- in the resulting `.jar` file, put them into `src/main/resources/`)

5. Edit `src/main/resources/plugins.config`
   ***Edit: not documented, and not extensively tested but this works and other attempts got errors in ImageJ that didn't run**
   ***      Needs to match what you put in pom.xml I think ***
   The first entry before the first comma "Process" means your plugin will be appended to the bottom of the Process menu in ImageJ
   The second entry is the name that will appear in the menu - I think it has to match what you put in pom.xml <name>
   The third entry, needs to be the concatenation of what you edited the <groupID> and <artefactID> to in pom.xml

6. Replace the contents of `README.md` with information about your project.
   ***Edit*** Well obviously here I've actually just edited the instructions

7. Make your initial
   [commit](https://help.github.com/en/desktop/contributing-to-projects/committing-and-reviewing-changes-to-your-project) and
   [push the results](https://help.github.com/en/articles/pushing-commits-to-a-remote-repository)!
   ***Edit: you don't NEED to do this yet, can be at any stage

8. ***Edit: moved down here, so it is in correct sequence order**
   You can now build your plugin.  The original instructions list 5 alternatives, but Eclipse is clearly the favoured route, and there are most help resources for Eclipse on imagej.net/develop

8.1 The 5 alternative build platforms

* In [Eclipse](http://eclipse.org), for example, it is as simple as
  _File &#8250; Import... &#8250; Existing Maven Project_.

* In [NetBeans](http://netbeans.org), it is even simpler:
  _File &#8250; Open Project_.

* The same works in [IntelliJ](http://jetbrains.net).

* If [jEdit](http://jedit.org) is your preferred IDE, you will need the
  [Maven Plugin](http://plugins.jedit.org/plugins/?MavenPlugin).

Die-hard command-line developers can use Maven directly by calling `mvn`
in the project root.

However you build the project, in the end you will have the `.jar` file
(called *artifact* in Maven speak) in the `target/` subdirectory.

To copy the artifact into the correct place, you can call
`mvn -Dscijava.app.directory=/path/to/ImageJ.app/`.
This will not only copy your artifact, but also all the dependencies. Restart
your ImageJ or call *Help &#8250; Refresh Menus* to see your plugin in the menus.

Developing plugins in an IDE is convenient, especially for debugging. To
that end, the plugin contains a `main` method which sets the `plugins.dir`
system property (so that the plugin is added to the Plugins menu), starts
ImageJ, loads an image and runs the plugin. See also
[this page](https://imagej.net/Debugging#Debugging_plugins_in_an_IDE_.28Netbeans.2C_IntelliJ.2C_Eclipse.2C_etc.29)
for information how ImageJ makes it easier to debug in IDEs.

***Edit new section***
8.2 Step-by-step in Eclipse

Make a new workspace.
  - to keep you sane, call it the same name as the GitHub repository
  - but it ***can't*** be the same location as the cloned GitHub repository on your local disk from step 2

As per the 8.1 original instructions, File > Import > Existing Maven Project
  - and navigate to the folder with the cloned Github repository, then just click through Finish
    with all defaults
  - NOTE: Use the File menu route (this works), not the links on the Welcome page (which will lead you down rabbit holes 
    of dialogs which won't find project files, won't allow you to just click finish, if they do complete,
    attempt to compile will give error messages which are gobbledegook to a newby just attempting to learn
    how to write a simple ImagerJ plugin)  This is super-frustrating for a beginner.
  - when the import progress bar (bottom right) has finished, close the welcome tab, Package Explorer tab will then appear
  
In the Eclipse Package Explorer, click on the twisty to open up > src/main/java
  - then select in the Package Explorer, the line that has opened up > com.my_company.imagej
    Note it still appears as com.my_company.imagej even though you changed that groupID in the pom.xml in step 3
    This is really confusing to the beginner, you expect pom.xml to be a configuration information file that Eclipse 
    will use to create the project structure, but it does appear to only be a configuration information file for
    the Maven build workflow, so you need to manually change this and other things as follows;
  - left click it (mouse still over commy_company.imagej), choose Refactor in the context sensitive menu that appears,
    and then Rename in the sub-menu that then appears.  
    In the dialog box that comes up, entre a names that matches the name you have specified in pom.xml and plugins.config
    The default Update Dependencies box is ticked - this works.
    When you click OK (or continue, can't recall now what the proceeed button is titled),
    a warning dialog will then appear, something to do with the main method:
    this can be ignorted, just click continue.
    
  - then, back in the Package Explorer, click the twisties to open up the next levels  > Process_Pixel.java and then > Process_pixels
  - select the Process_Pixels line ***not*** Process_Pixel.java, left click, Refactor, Rename as before
  - and again, choose the name to match pom.xml and plugins.config
  
  
Keeping the selection in the Project Explorer your newly renamed > thePlugin (not thePlugin.java, or higher up the tree)
From the menus, Run > Run will run the plugin, in a special debug version of ImageJ
  - but it ***won't*** create a *.jar so this is only for development and testing
  - to run it in your normal version of ImageJ, you need to build a *.jar,
    and have that *.jar copied to the /plugins folder of your normal ImageJ installation
  
To build a *.jar, Select the root entry of the Package Explorer, right click to get the context-senstive menu
and from that choose Run As and then the second entry on the list, Maven Build

An Edit Configuration dialog will appear.
  - You can just click Run with defaults, and it will build thePlugin-version number.jar in the /target folder
  - you can find the .jar in the /target sub folder of the place where you defined GitHub to clone it to
    (i.e. not in a subfolder of the workspace folder Eclipse creates).  Note that you ***won't*** see it in
    Eclipse Pckage Explorer target folder.
  - you can copy the *.jar manually to the /plugins folder of your normal ImageJ installation,
    and it will then appear as a new last itam on the Process menu (assuming you have at this point kep the
    Process_Pixels cosde, and the first entry in the plugins.config line remains Process)

To make Maven copy the plugin automatically, return to instructions in original README.mdin the file, but it should read

### Eclipse: To ensure that Maven copies the plugin to your ImageJ folder

1. Go to _Run Configurations..._
2. Choose _Maven Build_
3. Add the following parameter:
    - name: `scijava.app.directory`
    - value: `/path/to/ImageJ.app/`

***EDIT: with the correction, the value field should bethe path on your machine to the plugins subfolder of you ImageJ installation
e.g. lib://Documents/ImageJ/Plugins

ImageJ.app is the ImageJ2 executable, and this is an ImageJ 1 plugin

This ensures that the final `.jar` file will also be copied to
your ImageJ plugins folder everytime you run the Maven build.
