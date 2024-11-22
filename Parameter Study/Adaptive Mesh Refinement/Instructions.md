1. Welcome to the parameter study on adaptive mesh refinement!

2. Open your terminal back up. You should see something like this:

![image](https://github.com/user-attachments/assets/7a4e1067-bbce-4330-ba20-01835b96fa13)

or this:

![image](https://github.com/user-attachments/assets/617830db-b6c6-4e94-ae2c-a50e6f4eb343)

3. To figure out where you are, type: pwd

![image](https://github.com/user-attachments/assets/af8fa262-744e-4086-b619-4d009a6a179e)

4. You should be in this folder:

![image](https://github.com/user-attachments/assets/4ca3bf0a-1a3a-4d0d-8efd-f84d80fb43fe)

5. Let's create a new run.sh, type: vim runAdaptiveMeshRefinement.sh

![image](https://github.com/user-attachments/assets/ec2fd5be-043a-4a91-8cc3-e665ad22d57d)

6. Press enter:

![image](https://github.com/user-attachments/assets/fd92143b-5afa-41da-b2fd-e7246ade5344)

7. Press "i" to edit:

![image](https://github.com/user-attachments/assets/0f86590e-c379-468a-a986-bdbd72e006c3)

8. Copy and paste the following:

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

9. Before saving, play around with this line:

```
parthenon/mesh/numlevel=5 \
```

10. What happens if you change the "5" to a "3", or some other number? Try changing it to 3, or whatever number you decide on. Use the arrow keys to move around:

```
parthenon/mesh/numlevel=3 \
```    

![image](https://github.com/user-attachments/assets/f531be6f-4dfb-47d4-abca-138cfc83f0f6)

11. Press "Esc" on the keyboard:

![image](https://github.com/user-attachments/assets/0c573c29-b1bf-4367-b82f-c8fd955231b3)

12. Type: :wq

![image](https://github.com/user-attachments/assets/34051b44-a232-40c6-888d-2701f7ffcb0b)

13. Press "Enter" on the keyboard:

![image](https://github.com/user-attachments/assets/296be11f-d36d-434f-85b1-f037ddeed497)

14. To confirm that your file is there, type: ls

![image](https://github.com/user-attachments/assets/17bb169d-6148-420d-bbf4-394ea689a704)

15. Press "Enter" on the keyboard:

![image](https://github.com/user-attachments/assets/9ffca3a1-db2f-476c-8711-7907754cb316)

16. Type: sbatch runAdaptiveMeshRefinement.sh

![image](https://github.com/user-attachments/assets/1573326d-dd52-4ec1-8777-f6e71cba92e9)

17. Press "Enter" on the keyboard:

![image](https://github.com/user-attachments/assets/1247d1e7-3985-4e54-8da5-10d13728005d)

Your job number may be different

18. Check the status of your job by typing and entering: squeue -u USERNAME

Type your username instead of "USERNAME"

![image](https://github.com/user-attachments/assets/97c43706-ebb5-484b-be66-2934fe6bfc10)

19. Check the status of your job again if you'd like:

![image](https://github.com/user-attachments/assets/95189698-3013-4978-9e14-8b6ec179a52f)

20. This means that your job is done:

![image](https://github.com/user-attachments/assets/91448c62-55c2-4721-a17b-1e79d6ae2adb)

21. 

































































