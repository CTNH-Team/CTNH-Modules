# Create: New Horizon
## Submodules
CTNH-Core\
CTNH-Bio\
CTPP\
CTNH-Lib

## Getting Started

    git clone --recurse-submodules https://github.com/CTNH-Team/CTNH-Modules.git 

Then open the folder with IDEA, change submodules' branch into dev  
If there is something with submodules, try

    git submodule deinit --all -f
    git submodule update --init --recursive
    git submodule update --remote

To add new modules, use git to add them under modules,
and include them in settings.gradle  
VERY IMPORTANT: delete "finalizedBy 'reobfJar'" in submodule's build.gradle if you want other modules require it  
Now open build.gradle, roll down to the pre-defined tasks, press the run button on the left to run them     
NOTICE: If you use IDEA Gradle to run submodules (such as CTNH-Multi/modules/CTNH-Core/...),
it will use their folder as root dictionary,
which means that they can not find other modules as dependencies.

## Credits

https://github.com/sa-shiro/Minecraft-MultiProject-Template/tree/main/modules