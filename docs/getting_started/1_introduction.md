# Getting Started with JARVIS

![Jarvis Banner Getting Started](../assets/images/index/overview_wo_text.png){ .off-glb }

Welcome to our Getting Started Guide! This guide will teach you how to set up the three different parts of our toolbox and how to work with our example data. This will help you get an idea of all the steps required to build and use a 3D markerless motion capture setup. After working through this guide you will be ready to have a look at our **[Manual](../../manual/1_introduction)** and learn all the things necessary to build you own motion capture system from scratch.<br>
Note that this Guide is somewhat back to front. You will learn how to work with annotated data before we show you how to actually annotate it.<br>
<br>
One additional note, JARVIS uses two different formats for the annotated data. The one you will work with first is called the <span style="color:#63a31f">trainingset</span>. A trainingset is a finished set of annotations in the right format to be used by the JARVIS Pytorch module. The second one is called a <span style="color:#63a31f">dataset</span>, this is the format that is used by the AnnotationTool. We know this can be a bit confusing, but the two different formats are neccessary to make some features of JARVIS work.
