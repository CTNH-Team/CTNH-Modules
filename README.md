# Create: New Horizon
## Submodules
CTNH-Core\
CTNH-Bio\
CTPP\
CTNH-Lib

## Getting Started

    git clone --recurse-submodules https://github.com/CTNH-Team/CTNH-Modules.git 

Then open the folder with IDEA, change submodules' branch into dev  
If there is something wrong with submodules, try 

    git submodule deinit --all -f
    git submodule update --init --recursive
    git submodule update --remote

To add new modules, use git to add them under modules,
and include them in settings.gradle

## Credits

https://github.com/sa-shiro/Minecraft-MultiProject-Template/tree/main/modules