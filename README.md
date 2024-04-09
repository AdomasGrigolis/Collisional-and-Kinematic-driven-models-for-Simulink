# Collisional and Kinematic driven models for Simulink Robotic Toolbox
https://github.com/AdomasGrigolis/Collisional-and-Kinematic-driven-models-for-Simulink

# Introduction
The files provided are part of a project verifying capabilities of selected robotic manipulators in Biosafety Cabinet Applications. The specific files provided showcase how MATLAB’s Robotic Toolbox, utilizing Simulink environment, can be used to easily drive 3D manipulator models through inverse kinematics. Furthermore, a collisional model is provided. It can be used for self-collision or obstacle collision data. This simulation can provide multiple datasets including but not limited to workspace, manipulability etc. The specific given example uses Universal Robots UR3e manipulator, however, the model can be adapted for any manipulator.
# Important notes
The files do not contain necessary 3D modelling files, as these may be subject to copyright. The user must obtain their own models. Often these are provided at no cost online by manufacturers.
To import models directly from CAD files, SolidWorks (or other major 3D CAD platform) licence is required. Here, only information for importing models from SolidWorks is provided.
# How to use
1. Install prerequisite packages for Matlab Robotic Toolbox. More in [official website](https://uk.mathworks.com/products/robotics.html). Licenses are required.
2. [Follow step-by-step tutorial from Matlab](https://uk.mathworks.com/videos/import-solidworks-assemblies-into-simscape-multibody-1701670328083.html) on importing multibody files to Simulink. Note: only assembly files can be exported by this method.
3. Make a subsystem from the imported model. Copy and paste the subsystem into file of your choice (Kinematic driven or Collisional), replacing the existing UR3e subsystem. You can keep the subsystem for reference.
4. To add required inputs and outputs, go into newly added subsystem and open a revolute block parameter window. Under 'Actuation' change Torque -> Automatically computed, Motion -> Provided by input. Under Sensing, enable 'Position' setting. Joint limits can be provided as desired under 'Limits' section. This will create one additional input and output to the joint. Add outputs and inputs to the subsection. Repeat the process for all the joints.
5. To add collisional capabilities, open any solid body subsystem. Open solid body object, under properties expand 'Export' and tick 'Convex Hull'. This will create additional output from solid body object, close the properties. Add necessary output to the subsystem and go to parent. Add a necessary output to the parent subsystem as well. Repeat the process for all links. Note: 'Convex Hull' will fill in all spaces within a 3D object, so halo objects will not work and need to be provided as multiple objects instead.
6. Go to parent. The new subsystem should have all inputs and outputs required. Simply connect them as per the example, copy-pasting or deleting connections as per number of joints.
7. Open 'Forward Kinematics' block and within select 'Get Transform' block. Select the correct [RigidBodyTree](https://uk.mathworks.com/help/robotics/ug/rigid-body-tree-robot-model.html) for your manipulator. If you are using Kinematically driven model, same things needs to be done for 'Inverse Kinematics' block.
8. Use <code>importrobot('myrobot.slx')</code> [command](https://uk.mathworks.com/help/robotics/ref/importrobot.html) to generate necessary files. This needs to be done every time workspace is reset.

*Collisional model only

9. You will need to replace the links under 'GB_Collision_Box' subsystem to approariate mechanical files with .stp or .step file extension. Open the subsystem, then click the appropriate side of the box, for example 'Bottom'.
10. Open rigid body object and change the location of file as appropriate. Repeat this for all sides of the box.
11. Simply run the model run it. The simulation time determines how many points will be generated. Data is exported to a local file.

*Kinematically driven only*

9. Open 'IG' constant block and provide 1xN matrix of initial guesses (usually 0), where N is the number of joints of manipulator.
10. Open 'Signal Editor' block and add desired coordinates in space. The representation should correspond to 'Coordinate Transform Conversion' block where Euler Angles (ZYX) and Trajectory Vector are provided by default. More information on how to use signal editor can be found on [MATLAB's website](https://uk.mathworks.com/help/simulink/ug/insert-and-edit-signal-data.html). Note: In newer builds of Matlab, in 'Signal Editor' options you may wish to enable 'Interpolate data' setting which will make signals continuous.
11. The trajectories can be observed in 'Plotter' subsection scopes or in 'Simulation Mechanics View'.

# Potential issues
- If within SolidWorks you do not see the option to export Multibody Link, ensure you are working with assembly file and not part.
- The simulations can run very slow. Relatively powerful hardware is needed. If you are running Collisional model, disable Mechanics Explorer. If you are working with Kinematic model, you can change the simulation parameters in 'Inverse Kinematics' block under 'Solver Parameters'. Usually, reducing error tolerances helps. Additionally, try to use built-in [Simulink Accelerator](https://uk.mathworks.com/help/simulink/ug/how-the-acceleration-modes-work.html) (BUT DO NOT USE Rapid Acceleraotr).
