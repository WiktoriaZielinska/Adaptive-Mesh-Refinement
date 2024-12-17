# Welcome to the parameter exercise on adaptive mesh refinement!

## Steps 1 - 3: Terminal Setup and File Structure

### 1. Open your terminal back up. You should see something like this:

![image](https://github.com/user-attachments/assets/7a4e1067-bbce-4330-ba20-01835b96fa13)

or this:

![image](https://github.com/user-attachments/assets/617830db-b6c6-4e94-ae2c-a50e6f4eb343)

### 2. To figure out where you are, type: ```pwd```

![image](https://github.com/user-attachments/assets/af8fa262-744e-4086-b619-4d009a6a179e)

### 3. You should be in this folder:

![image](https://github.com/user-attachments/assets/4ca3bf0a-1a3a-4d0d-8efd-f84d80fb43fe)

---

## Steps 4 - 10: Organizing Files

### 4. Let's move our current files into a new folder. Type: ```mkdir originalFiles```

![image](https://github.com/user-attachments/assets/5e8e7ed5-8b9c-4296-baaf-4bd0ee88d426)

### 5. Press ```Enter``` on the keyboard:

![image](https://github.com/user-attachments/assets/1c5ada82-c42f-420c-8bbb-57504c0449c3)

### 6. Type: ```ls```

![image](https://github.com/user-attachments/assets/818ee114-ac44-439e-ab3a-bd43907abee1)

### 7. Press ```Enter``` on the keyboard:

![image](https://github.com/user-attachments/assets/291c3c84-01f6-4421-83ba-e667985e241d)

### 8. You should see the new folder that we created. Let's move our files into that folder. Type: ```mv parthenon.out0.0000* originalFiles/```

![image](https://github.com/user-attachments/assets/60976312-ea6a-48ea-8927-b92b514e2332)

### 9. Press ```Enter``` on the keyboard:

![image](https://github.com/user-attachments/assets/47e99af4-0e4f-4b82-847a-af88857faf44)

### 10. Enter the ```ls``` command and press ```Enter``` to see that your files have been moved:

![image](https://github.com/user-attachments/assets/b5bf183f-bc6b-47ea-9a72-5242f72c8439)

---

## Steps 11 - 16: Moving Additional Files

### 11. Let's move the 2 last files over. Type: ```mv AthenaPK-2798704.out originalFiles/```

![image](https://github.com/user-attachments/assets/4ab00351-d630-4a86-9e06-986cd903c319)

### 12. Press ```Enter``` on the keyboard:

![image](https://github.com/user-attachments/assets/c9e297d4-5d2b-47f1-a376-b48678392115)

### 13. Enter the ```ls``` command and press ```Enter``` to see that your file has been moved:

![image](https://github.com/user-attachments/assets/96df0f55-d383-49e1-906e-320c4d2b310a)

### 14. Type: ```mv run.sh originalFiles/```

![image](https://github.com/user-attachments/assets/81ce4f15-4afb-4d78-a685-a4d220fea341)

### 15. Press ```Enter``` on the keyboard:

![image](https://github.com/user-attachments/assets/a07e71ed-593c-4694-b2c9-520f8eabf7b5)

### 16. Enter the ```ls``` command and press ```Enter``` to see that your file has been moved:

![image](https://github.com/user-attachments/assets/0e0ccbf0-1253-4af5-baea-577b7f6acb77)

We will not worry about the new folder that we just created, unless we want to go back to it to review it.

---

## Steps 17 - 26: Creating and Verifying the New Run Script

### 17. Let's create a new run.sh, type: ```vim runAdaptiveMeshRefinement.sh```

![image](https://github.com/user-attachments/assets/ec2fd5be-043a-4a91-8cc3-e665ad22d57d)

### 18. Press ```Enter```:

![image](https://github.com/user-attachments/assets/fd92143b-5afa-41da-b2fd-e7246ade5344)

### 19. Press ```i``` to edit:

![image](https://github.com/user-attachments/assets/0f86590e-c379-468a-a986-bdbd72e006c3)

### 20. Copy and paste the following:

```
#!/bin/bash


#SBATCH -A GEN243
#SBATCH -J AthenaPK
#SBATCH -o %x-%j.out
#SBATCH -t 00:10:00
#SBATCH -q debug
#SBATCH -N 1


module load cpe/23.09
module load PrgEnv-amd
module load craype-accel-amd-gfx90a
module load cmake
module load hdf5
module load ums
module load ums022
module -t list


date

export MPICH_GPU_SUPPORT_ENABLED=1

srun -N1 -n8 -c7 --ntasks-per-node=8 --gpus-per-node=8 --gpu-bind=closest ./athenaPK \
    -i /ccs/home/wiktoria_zielinska/athenapk/inputs/blast_image.in \
    problem/blast/input_image=/lustre/orion/stf243/world-shared/holmenjk/image.pbm \
    problem/blast/pressure_ratio=350 \
    parthenon/time/nlim=500 \
    parthenon/mesh/numlevel=5 \
    parthenon/mesh/nx1=32 parthenon/mesh/nx2=48 parthenon/mesh/nx3=1 \
    parthenon/meshblock/nx1=4 parthenon/meshblock/nx2=4 parthenon/meshblock/nx3=1 \
    parthenon/output0/dt=0.05 \
    parthenon/output0/variables=cons
```


**NOTES:**

You’ll need to replace the srun parameter pointing to the input with the path to your input (i.e.,  -i /<path-to-your-athenapk-folder>/athenapk/inputs/blast_image.in \)

This is the line that you will need to change: 

```
srun -N1 -n8 -c7 --ntasks-per-node=8 --gpus-per-node=8 --gpu-bind=closest ./athenaPK \
    -i /ccs/home/wiktoria_zielinska/athenapk/inputs/blast_image.in \
```

### 21. Before saving, play around with this line:

```
parthenon/mesh/numlevel=5 \
```

### 22. What happens if you change the ```5``` to a ```3```, or some other number? Try changing it to ```3```, or whatever number you decide on. Use the arrow keys to move around:

```
parthenon/mesh/numlevel=3 \
```    

![image](https://github.com/user-attachments/assets/f531be6f-4dfb-47d4-abca-138cfc83f0f6)

### 23. Press ```Esc``` on the keyboard:

![image](https://github.com/user-attachments/assets/0c573c29-b1bf-4367-b82f-c8fd955231b3)

### 24. Type: ```:wq```

![image](https://github.com/user-attachments/assets/34051b44-a232-40c6-888d-2701f7ffcb0b)

### 25. Press ```Enter``` on the keyboard:

![image](https://github.com/user-attachments/assets/296be11f-d36d-434f-85b1-f037ddeed497)

### 26. To confirm that your file is there, type: ```ls```

![image](https://github.com/user-attachments/assets/17bb169d-6148-420d-bbf4-394ea689a704)

---

## Steps 27 - 31: Job Submission and Visualization

### 27. Press ```Enter``` on the keyboard:

![image](https://github.com/user-attachments/assets/ca2ac6de-fbd7-4558-9b23-4e601c25d609)

### 28. Type: ```sbatch runAdaptiveMeshRefinement.sh```

![image](https://github.com/user-attachments/assets/1573326d-dd52-4ec1-8777-f6e71cba92e9)

### 29. Press ```Enter``` on the keyboard:

![image](https://github.com/user-attachments/assets/c031f7d5-bf57-40ac-af65-bdf0fc51bbb6)

Your job number may be different

### 30. Check the status of your job by typing and entering: ```squeue -u USERNAME```

Type your username instead of "USERNAME"

![image](https://github.com/user-attachments/assets/9c250fcf-626b-4091-8557-51ae27d9fdca)

### 31. Check the status of your job again. No time means that your job is done:

![image](https://github.com/user-attachments/assets/24ece51b-d773-4214-b8ef-3f465d5c1aec)

---

## Steps 32 - 45: Visualizing the Results in VisIt

### 32. Enter the ```ls``` command and press ```Enter``` to see all of the new files that have been created:

![image](https://github.com/user-attachments/assets/efa72c19-61d8-459e-83ac-a06a5b8f2474)

### 33. It is now time to visualize our files in VisIt. Open up the two VisIt windows:

![image](https://github.com/user-attachments/assets/6e66d843-7dc6-41e3-a76a-20622d6881fd)

### 34. Click ```Open```:

![image](https://github.com/user-attachments/assets/f01402b0-2871-433f-b465-f94da5c46afb)

### 35. You should now see this screen:

![image](https://github.com/user-attachments/assets/5f813dd3-cacd-4ae9-80e9-6fbda42141ae)

### 36. Scroll down and select the ```xdmf``` files:

![image](https://github.com/user-attachments/assets/73213ffe-a576-4422-bc77-81f81579a000)

### 37. Press ```OK```. You should now see this screen come up:

![image](https://github.com/user-attachments/assets/26c9a230-d227-4630-a3b9-8dd9fcd0865d)

### 38. Change the Project ID and time limit again, then press ```OK```:

![image](https://github.com/user-attachments/assets/3e2ce48f-6edc-41b7-9948-b9c905efd6b1)

### 39. You should see this screen:

![image](https://github.com/user-attachments/assets/b9800f5d-d931-4233-9f4b-b1f27e18c2bc)

### 40. Delete the second variable that we added, which was ```Pseudocolor - cons_density```:

![image](https://github.com/user-attachments/assets/e058b430-f6f2-44d1-8989-321ed9f59e91)

### 41. Click ```Draw``` now:

![image](https://github.com/user-attachments/assets/9aac8345-76f0-4bc1-b3b2-829190f85103)

### 42. You should now see this screen:

![image](https://github.com/user-attachments/assets/a0717fff-613c-4d80-ae12-c59443319411)

### 43. Notice how we can now barely tell what our image is? That is because we changed the mesh from ```5``` to ```3``` (or whatever number you changed it to). Also notice how we have way more images now, and it runs faster?

![image](https://github.com/user-attachments/assets/2b4f5164-a17e-464b-9174-633bc8bcbbec)

### 44. Play around with the controls and what variables we can add, like ```Pseudocolor - cons_density```. Try to understand how changing the mesh affects our visualization. Think about the way our visualization looks, how fast it runs, how many images we get, etc. 

![image](https://github.com/user-attachments/assets/c3ac72cc-4e47-488c-99ca-890ae7fcee89)

### 45. Go ahead and repeat this instruction set with a different mesh level, or move onto the next parameter study. Also note, extreme values may break the visualization! Try to find out what happens if you try negative numbers, or really big numbers. Maybe it will crash? Maybe it will take a very long time to load? Experiment and find out!

