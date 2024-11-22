1. Welcome to the parameter study on adaptive mesh refinement!

2. Open your terminal back up. You should see something like this:

![image](https://github.com/user-attachments/assets/7a4e1067-bbce-4330-ba20-01835b96fa13)

or this:

![image](https://github.com/user-attachments/assets/617830db-b6c6-4e94-ae2c-a50e6f4eb343)

3. To figure out where you are, type: pwd

![image](https://github.com/user-attachments/assets/af8fa262-744e-4086-b619-4d009a6a179e)

4. You should be in this folder:

![image](https://github.com/user-attachments/assets/4ca3bf0a-1a3a-4d0d-8efd-f84d80fb43fe)

5. Let's move our current files into a new folder. Type mkdir originalFiles

![image](https://github.com/user-attachments/assets/5e8e7ed5-8b9c-4296-baaf-4bd0ee88d426)

6. Press "Enter" on the keyboard:

![image](https://github.com/user-attachments/assets/1c5ada82-c42f-420c-8bbb-57504c0449c3)

7. Type: ls

![image](https://github.com/user-attachments/assets/818ee114-ac44-439e-ab3a-bd43907abee1)

8. Press "Enter" on the keyboard:

![image](https://github.com/user-attachments/assets/291c3c84-01f6-4421-83ba-e667985e241d)

9. You should see the new folder that we created. Let's move our files into that folder. Type: mv parthenon.out0.0000* originalFiles/

![image](https://github.com/user-attachments/assets/60976312-ea6a-48ea-8927-b92b514e2332)

10. Press "Enter" on the keyboard:

![image](https://github.com/user-attachments/assets/47e99af4-0e4f-4b82-847a-af88857faf44)

11. Enter the "ls" command and press "Enter" to see that your files have been moved:

![image](https://github.com/user-attachments/assets/b5bf183f-bc6b-47ea-9a72-5242f72c8439)

12. Let's move the last file over. Type: mv AthenaPK-2798704.out originalFiles/

![image](https://github.com/user-attachments/assets/4ab00351-d630-4a86-9e06-986cd903c319)

13. Press "Enter" on the keyboard:

![image](https://github.com/user-attachments/assets/c9e297d4-5d2b-47f1-a376-b48678392115)

14. Enter the "ls" command and press "Enter" to see that your file has been moved:

![image](https://github.com/user-attachments/assets/96df0f55-d383-49e1-906e-320c4d2b310a)

We will not worry about the new folder that we created unless we want to go back to see its contents.

15. Let's create a new run.sh, type: vim runAdaptiveMeshRefinement.sh

![image](https://github.com/user-attachments/assets/ec2fd5be-043a-4a91-8cc3-e665ad22d57d)

16. Press enter:

![image](https://github.com/user-attachments/assets/fd92143b-5afa-41da-b2fd-e7246ade5344)

17. Press "i" to edit:

![image](https://github.com/user-attachments/assets/0f86590e-c379-468a-a986-bdbd72e006c3)

18. Copy and paste the following:

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


NOTES:

You’ll need to replace the srun parameter pointing to the input with the path to your input (i.e.,  -i /<path-to-your-athenapk-folder>/athenapk/inputs/blast_image.in \)

This is the line that you will need to change: 

```
srun -N1 -n8 -c7 --ntasks-per-node=8 --gpus-per-node=8 --gpu-bind=closest ./athenaPK \
    -i /ccs/home/wiktoria_zielinska/athenapk/inputs/blast_image.in \
```

19. Before saving, play around with this line:

```
parthenon/mesh/numlevel=5 \
```

20. What happens if you change the "5" to a "3", or some other number? Try changing it to 3, or whatever number you decide on. Use the arrow keys to move around:

```
parthenon/mesh/numlevel=3 \
```    

![image](https://github.com/user-attachments/assets/f531be6f-4dfb-47d4-abca-138cfc83f0f6)

21. Press "Esc" on the keyboard:

![image](https://github.com/user-attachments/assets/0c573c29-b1bf-4367-b82f-c8fd955231b3)

22. Type: :wq

![image](https://github.com/user-attachments/assets/34051b44-a232-40c6-888d-2701f7ffcb0b)

23. Press "Enter" on the keyboard:

![image](https://github.com/user-attachments/assets/296be11f-d36d-434f-85b1-f037ddeed497)

24. To confirm that your file is there, type: ls

![image](https://github.com/user-attachments/assets/17bb169d-6148-420d-bbf4-394ea689a704)

25. Press "Enter" on the keyboard:

![image](https://github.com/user-attachments/assets/9ffca3a1-db2f-476c-8711-7907754cb316)

26. Type: sbatch runAdaptiveMeshRefinement.sh

![image](https://github.com/user-attachments/assets/1573326d-dd52-4ec1-8777-f6e71cba92e9)

27. Press "Enter" on the keyboard:

![image](https://github.com/user-attachments/assets/1247d1e7-3985-4e54-8da5-10d13728005d)

Your job number may be different

28. Check the status of your job by typing and entering: squeue -u USERNAME

Type your username instead of "USERNAME"

![image](https://github.com/user-attachments/assets/97c43706-ebb5-484b-be66-2934fe6bfc10)

29. Check the status of your job again if you'd like:

![image](https://github.com/user-attachments/assets/95189698-3013-4978-9e14-8b6ec179a52f)

30. This means that your job is done:

![image](https://github.com/user-attachments/assets/91448c62-55c2-4721-a17b-1e79d6ae2adb)

31. 

































































