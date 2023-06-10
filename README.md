# ZYNQ assignment for ZedBoard

## Getting started

1) Download [Vivado 2020.2](https://www.xilinx.com/support/download/index.html/content/xilinx/en/downloadNav/vivado-design-tools/archive.html). 

    During installation, select Vitis & Vivado. For bord support, only Zynq 7000 is required (no Ultrascale, Ultrascale+,..). After installation, **do not** update to version 2022.2.2, since Vivado is generally not downward compatible, which limits collaboration compatibility.

    The Vivado (IDE for HW & RTL design for Xilinx FPGAs) and Vitis (co-development of software for Vivado projects) design flow steps in general are:

    - Create a Vivado Project: Start by creating a Vivado project to define the FPGA hardware platform. This involves creating a block design, adding IP blocks (if required), and configuring the desired FPGA settings. Ensure that you enable the necessary interfaces (such as AXI or GPIO) for communication with the processing system (PS).

    - Generate the Bitstream: Once your Vivado project is complete, generate the bitstream file (.bit) that contains the configuration data for the FPGA fabric.

    - Export Hardware Description: After generating the bitstream, export the hardware description file (.xsa). This file contains the hardware design information required for software development using Vitis.

    - Create a Vitis Application Project: Launch Vitis and create a new application project. Specify the target hardware by importing the exported hardware description file (.xsa) from Vivado. Select the appropriate processor and configure the software platform settings.

    - Develop the Software Application: Write your software application code in C/C++ or OpenCL within the Vitis project. This code will run on the embedded processor within the FPGA.

    - Build the Application: Build the software application project in Vitis. This step compiles the code, links it with the necessary libraries, and generates an executable file.

    - Create SD Card Image: Once the software application is built, create an SD card image that includes the necessary files for booting the ZedBoard. This image typically contains the bitstream, the software application executable, and other required files.

    - Program the FPGA: Program the FPGA on the ZedBoard using the generated bitstream file. This step configures the FPGA fabric with the desired hardware design.

    - Boot the ZedBoard: Insert the SD card with the created image into the ZedBoard and power it on. The board will boot the software application, which will run on the embedded processor.

2) Using this repository
- Opening the Vivado project: After cloning this repo, open Vivado and select `Open Project` in the Quick Start menu. Then select `<path to repo>/graycode/graycode_vivado_prj/graycode_vivado_prj.xpr` to open the project.
- Using Vitis: Set the workspace directory to `<<path to repo>/workspace/>`.

## Notable Bugs found in Vivado

On Ubuntu: Block design validation only works when **Region and Language** is set to US. 

1) Go to Setting -> Region and Language -> Formats

2) Choose United States as input to avoid Errors during block validation.

## Basic Git usage:

- To create a local copy of a repository use `git clone`, e.g.

    ```bash
    git clone https://github.com/dippmatt/zynq-embedded-fpga-tuwien.git
    ```

- ### Branch management: 
    After cloning, you have a local copy of the `default` branch of the remote repository. This branch is usually is called `main` or `master`. This branch is a good starting point for all developers, who want to use or code on the project. This branch should always be kept clean, including it's commit history, which we will discuss later. 

    If you are a developer, you therefore don't edit files on the default branch directly and always create your own branch before editing source code, that should be commited to the remote repository later. You can create a new feature branch using:

    ```bash
    git checkout -b <my feature branch name>
    ```

    **Note:** Before creating a new branch, it is best practice to check if any changes have been added to the remote default branch, after you cloned the repository. Use `git pull` to update your local copy of the repository.

    The option `-b` is used to create a new branch. If the branch you want to work on already exists, you can omit this option.

    If you are not sure which branch you are on, use 
    ```bash
    git status
    ``` 
    to check. Now you can edit files as you want. After making changes that you can easily describe in a few words, you can create a **commit**, which is used to track the history of development and document your changes for your future self and other developers. 
    
    But first we need to tell git which files are actually intended to be commited. Either by using `git add <filename>`, `git add <directory>` for a whole new directory or
    ```bash
    git add *
    ``` 
    to add all modified or new, untracked files.

    **Note: Make sure you only add source code.** Compiled binarys, object files, bitstreams,.. etc. should not be commited to a repository! Github is not a cloud service and should not be used as one.

    You also don't need to delete those files, every time you make a commit. Git supports a convenient file to exclude files you don't want to commit. It is usually not visible by default, called `.gitignore` and found repositories top level directory. Just add any file/directory paths of files you want to ignore to the `.gitignore` file. You can also filter by filetype, e.g. if you want to ignore all bitstreams add `*.bit`. 
    
    More details on .gitignore [here](https://www.atlassian.com/git/tutorials/saving-changes/gitignore).

    Now you can commit your changes:

    ```bash
    git commit
    ``` 
    [Turorial: How to write good commit messages](https://cbea.ms/git-commit/).

    You are almost done. The last step is to upload your changes to the remote repository, which we can do with the `push` command.

    ```bash
    git push origin <your-local-branch-name>
    ``` 