![Create new chip](https://github.com/JuliProg/MT29F2G08ABAEAH4/workflows/Create%20new%20chip/badge.svg?event=repository_dispatch)
![ChipUpdate](https://github.com/JuliProg/MT29F2G08ABAEAH4/workflows/ChipUpdate/badge.svg)
# Join the development of the project ([list of tasks](https://github.com/users/JuliProg/projects/1))


# MT29F2G08ABAEAH4
Implementation of the MT29F2G08ABAEAH4 chip for the JuliProg programmer

Dependency injection, DI based on MEF framework is used to connect the chip to the programmer.

<section class = "listing">

# Chip parameters
```c#


        //--------------Warning!!!!!!!!!!!!!!!-------------------------------------------
        //
        //   The first operation must be a command
        //   Reset  !!!
        //
        //--------------------Vendor Specific Pin configuration---------------------------

        //  VSP1(38pin) - NC    
        //  VSP2(35pin) - NC
        //  VSP3(20pin) - NC



        ChipAssembly()
        {
            myChip.devManuf = "Micron";
            myChip.name = "MT29F2G08ABAEAH4";
            myChip.chipID = "2CF1809502";      // device ID - 2Ch F1h 80h 95h 02h (Micron-MT29F2G08ABAEAH4WP-IT_D-datasheet.pdf page 33)

            myChip.width = Organization.x8;    // chip width - 8 bit
            myChip.bytesPP = 2048;             // page size - 2048 byte (2Kb)
            myChip.spareBytesPP = 64;          // size Spare Area - 64 byte
            myChip.pagesPB = 64;               // the number of pages per block - 64 
            myChip.bloksPLUN = 1024;           // number of blocks in CE - 1024
            myChip.LUNs = 1;                   // the amount of CE in the chip
            myChip.colAdrCycles = 2;           // cycles for column addressing
            myChip.rowAdrCycles = 2;           // cycles for row addressing 
            myChip.vcc = Vcc.v3_3;             // supply voltage

```
# Chip operations
```c#


            //------- Add chip operations    https://github.com/JuliProg/Wiki#command-set----------------------------------------------------

            myChip.Operations("Reset_FFh").
                   Operations("Erase_60h_D0h").
                   Operations("Read_00h_30h").
                   Operations("PageProgram_80h_10h");

```
# Chip registers (optional)
```c#


            //------- Add chip registers (optional)----------------------------------------------------

            myChip.registers.Add(                   // https://github.com/JuliProg/Wiki/wiki/StatusRegister
                "Status Register").
                Size(1).
                Operations("ReadStatus_70h").
                Interpretation("SR_Interpreted").
                UseAsStatusRegister();



            myChip.registers.Add(                  // https://github.com/JuliProg/Wiki/wiki/ID-Register
                "Id Register").
                Size(5).
                Operations("ReadId_90h");               
                //Interpretation(ID_interpreting);
            
            myChip.registers.Add(                  // https://github.com/JuliProg/Wiki/wiki/OTP
                "OTP memory area").
                Size(63360).
                Operations("OTP_Mode_On_v1").
                Operations("OTP_Mode_Off_v1");    

```
# Interpretation of ID-register values ​​(optional)
```c#


        public string ID_interpreting(Register register)   
        
```
</section>












footer
