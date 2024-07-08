1. Take note of your supercomputer and the version using this link: https://docs.olcf.ornl.gov/software/viz_tools/visit.html

![image](https://github.com/WiktoriaZielinska/Adaptive-Mesh-Refinement/assets/112288108/ff7d0f6b-6c76-40f9-9273-de81b7159184)

2. Go to this website: https://visit-dav.github.io/visit-website/releases-as-tables/ and look for the correct version. Is your laptop a Windows, Mac, or Linux? In my case, I was using a Windows laptop, so I installed Version 3.3.3 from Mar 2023, which was actually not the latest version.

![image](https://github.com/WiktoriaZielinska/Adaptive-Mesh-Refinement/assets/112288108/b11e6d85-1d74-40b1-98cf-d09dcce05eb4)

3. Once that finishes installing, launch it.

4. Go through the launching process, and if prompted, choose “Oak Ridge National Laboratory” as the lab.

5. Simply follow this tutorial for the rest of the instructions on how to set up VisIt and a tutorial on how to use it: https://github.com/olcf/dva-training-series/tree/main/visit#download

NOTES:

Instead of using an already existing profile, simply create your own and enter this information:

Host nickname: Frontier

Remote host name: frontier.olcf.ornl.gov

Host name aliases: login#

Path to VisIt installation: /sw/frontier/ums/ums022/linux-sles15-zen3/gcc-11.2.0/visit-3.3.3-zfoh2caq5tbshlvtujditymjizstvewe/

Username: ENTER THE SAME USERNAME THAT YOU’VE BEEN USING TO LOG INTO FRONTIER

Tunnel data connections through SSH: have this checked to yes

![image](https://github.com/WiktoriaZielinska/Adaptive-Mesh-Refinement/assets/112288108/023f09d5-1e0d-484d-925f-4eaaa460b508)

6. Click on “Launch Profiles: and enter this information:

![image](https://github.com/WiktoriaZielinska/Adaptive-Mesh-Refinement/assets/112288108/910e3a84-64df-4fa7-a4f6-c858097665bd)

Make sure that everything is checked to yes except the last check as seen above

Parallel launch method: srun

Partition / Pool / Queue: batch

Number of processors: 1

Number of nodes: 1

Bank / Account: GEN243 (OR WHATEVER YOUR PROJECT ID IS)

Time Limit: 00:15:00

7. Click apply.

8. Then you may close out of the window titled “Host Profiles”

![image](https://github.com/WiktoriaZielinska/Adaptive-Mesh-Refinement/assets/112288108/d10d0306-4a01-4998-b5da-8e8bbb42e2c4)

9. Continue on with the rest of the VisIt tutorial until you feel comfortable, try to make at least one visualization: https://github.com/olcf/dva-training-series/tree/main/visit#download

10. Congratulations! You are ready to move onto the next step!




























