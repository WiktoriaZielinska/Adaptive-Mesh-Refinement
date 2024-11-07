1. After submitting your job and it finishes, you are now ready to open VisIt and see your simulation.

2. Launch the VisIt app:

![image](https://github.com/user-attachments/assets/430563e8-08b7-41bc-a14d-9947ecab0eec)

3. Choose the correct Host Profile, this should be "ORNL_Andes". Click "Post":

![image](https://github.com/user-attachments/assets/13ca3a34-9e17-409f-8a8e-b58ce887f13c)

4. Now, click on "Open".

![image](https://github.com/WiktoriaZielinska/Adaptive-Mesh-Refinement/assets/112288108/0eed222e-4f77-4357-8879-5bb1a7309abc)

5. You should now see this screen:

![image](https://github.com/WiktoriaZielinska/Adaptive-Mesh-Refinement/assets/112288108/c233aa63-f935-4b3f-b720-b50e9fb63a1c)

6. Change "Host" from "localhost" to "ORNL_Andes".

The screen will grey out and it will look something like this:

![image](https://github.com/user-attachments/assets/2926e477-3964-402a-b9e1-855af5d17015)

You should now see this pop up at some point:

![image](https://github.com/user-attachments/assets/9fa83855-8012-442a-96d1-26a5bfc605a3)

7. Enter your 6 digit PIN and your RSA Token PIN:

![image](https://github.com/user-attachments/assets/e5ec182a-fcb5-400b-aaec-f43684d2fb62)

You should now see this:

![image](https://github.com/user-attachments/assets/cc155f7b-c2cb-4fa4-abd9-116328c3507a)

You should also see this if all went well:

![image](https://github.com/user-attachments/assets/de228703-ee18-44ed-b824-75df821cc5b0)

Going back to VisIt, you should see this screen:

![image](https://github.com/user-attachments/assets/7ee0e137-3760-446c-8dc2-475287a961eb)

8. Click on "athenapk" in the Directories:

![image](https://github.com/user-attachments/assets/cc5b7928-22da-42a4-a450-9b6d806efb58)

9. Click on "build" in the Directories:

![image](https://github.com/user-attachments/assets/5eb5590f-0be2-4771-a464-38b0124aa0df)

10. Click on "bin" in the Directories:

![image](https://github.com/user-attachments/assets/9bc9a0db-ee74-4877-92cb-a3b7fe0f935e)

You should now see this screen:

![image](https://github.com/user-attachments/assets/b9b6c3ea-2aae-41f5-8492-8adffc4020d2)

11. In Files, scroll down and click on the parthenon files that have "xdmf" in them. Then click on "OK":

![image](https://github.com/user-attachments/assets/9e84f3db-606c-468e-a47d-cd6cfe3bc38f)

12. Look for this screen, and type in your Project ID into "Bank", then click on "OK":

![image](https://github.com/user-attachments/assets/d393d7b3-fb91-4905-8d92-2de0c700c386)

13. This screen will pop up, and it will shortly disappear when VisIt it ready to visualize your data:

![image](https://github.com/user-attachments/assets/58aae095-c172-4280-a484-783aad196da7)

13. When that screen disappears, open up these two screens again:

![image](https://github.com/user-attachments/assets/84b1ee19-eb4d-4b78-9b0b-bba65b6f259a)

14. Click on "Add":

![image](https://github.com/user-attachments/assets/5fa3a448-d4d3-458c-97d9-65af931920fa)

15. You can choose from any of the options below, but let's focus on this one for now. Click on "Mesh" and then "Mesh" again:

![image](https://github.com/user-attachments/assets/429298f7-76ce-4312-85ca-db9038815ae9)

16. You should now see this screen:

![image](https://github.com/user-attachments/assets/21e4aca8-ed18-417c-91ef-f511e1e27aea)

17. Click on "Draw":

![image](https://github.com/user-attachments/assets/032164da-c375-434e-9a8b-3628c7e007e3)

18. It will take a few seconds to load:

![image](https://github.com/user-attachments/assets/6b6ad6d6-0f3b-4c37-8e49-ef4abebe3517)

19. This is what you will see once it has fully loaded:

![image](https://github.com/user-attachments/assets/c5495115-ccd3-4256-9bbe-73ac31dc3e0f)

20. Click on the right play button next to the square in the middle. This is above the "Draw" button:

![image](https://github.com/user-attachments/assets/b0ce6afa-67c4-432c-b89e-e259923668c9)

21. It will take some time for VisIt to load the next time step. You can see that it is done loading based on the information in the bottom left corner. You can see the next time step under "Window 1" where it says "00001" now:

![image](https://github.com/user-attachments/assets/5fd53ddd-cdd2-4e69-be20-3e98ac2ed315)

22. VisIt will continue to go through each time step until it gets to the end or until you stop it with the center square button to the left of the right play button:

![image](https://github.com/user-attachments/assets/797e1152-a81e-422d-bfc1-861f4e5fae78)

23. To start the visualization from the beginning, click on the furthest play button on the left:

![image](https://github.com/user-attachments/assets/8650ee9c-c4e3-4039-ae99-c2aab1e4faab)

24. VisIt will automatically continue with the visualization if you haven't clicked on the center square yet:

![image](https://github.com/user-attachments/assets/44f6ecb7-a0d7-42df-90bf-e4411f276209)

25. Clicking on the left play button to the left of the square will simply make the visualization go backwards from the time step it is currently at:

![image](https://github.com/user-attachments/assets/5f2559e7-1566-4b5b-97ec-24e633f02deb)

26. Now let's try adding another variable. Click on "Add" then click on "Pseudocolor" then click on "cons_density":

![image](https://github.com/user-attachments/assets/2d3399b7-5995-4a60-a09b-14b974cca4af)

27. After you do that and click on "Draw", you should now see this:

![image](https://github.com/user-attachments/assets/64900267-2ef6-48f3-bbcc-418c1709c7c8)

28. If you want to see how this visualization looks like, then go ahead and play around with the play buttons:

![image](https://github.com/user-attachments/assets/e644e529-1c33-4b94-a60c-3eb6a955b6ce)



















