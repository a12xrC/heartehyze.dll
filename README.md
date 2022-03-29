# heartehyze.dll
![MOSHED-2022-3-22-12-59-34(1)](https://user-images.githubusercontent.com/102240206/160683020-38335246-1d36-4139-bd08-51c252ec94fe.jpg)

PE infector
this is another one of my open source malwares
First of all we will add a new empty PE section.Given a file path,we read the entire file,check its DOS signature for a valid PE,check if it is a x86 executable (important since we will only inject x86 opcode into this section),check if the section we wanna create doesnt already exist and if all checks out,we create a new section with a given size and characteristics.Comments are in the code and it is better explained there.
This all is done by the AddSection function.It is good to mention that the method explained here uses the file on the disk,without mapping it into memory and applying read/write operations there.It highly relies on file pointers.

The second and the toughest part is adding the code into the newly created section.
So,basecaly what i want to do to prove this concept is this:

    Add a new section
    Retrive and save the original entry point from the Obtional Header (the address from witch the actual program starts)
    We change the OEP to point to our new section,so when the user opens the executable,the program starts at our code,does whatever we want BEFORE any other original code,then returns to its original entry point so the program will run as usual! In this case,i will pop a message box with the string "Hacked" in it!

    Add a new section
    Retrive and save the original entry point from the Obtional Header (the address from witch the actual program starts)
    We change the OEP to point to our new section,so when the user opens the executable,the program starts at our code,does whatever we want BEFORE any other original code,then returns to its original entry point so the program will run as usual! In this case,i will pop a message box with the string "Hacked" in it!

IMPORTANT
Since the loader loads kernel32 at a different address each time the computer is restarted,we will need to DYNAMICALY retrive its base address,from PEB,so we can look for a function in the export table named LoadLibraryA, call it with "hearteyes.dll" as argument,and later retrieve the function address of MessageBoxA from user32.dll using a call to GetProcAddress.
ALL OF THIS MUST BE DONE FROM WITHIN THE NEW PE SECTION WE'VE JUST CREATED !!!
