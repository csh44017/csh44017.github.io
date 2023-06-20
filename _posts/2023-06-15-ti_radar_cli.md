---
title:  "TI people counting CLI 함수 정리"
excerpt: "TI 레이더 펌웨어에서 구현된 cli 함수 정리"

categories:
  - Radar
tags:
  - [Radar, Configuration, Command, CLI]

toc: true
toc_sticky: true

date: 2023-06-15
last_modified_at: 2023-06-20
---

## People Counting CLI  
**overhead_3d_people_count_6843_mss/mss/mss_main.c**  
- CLI task 시작  
  main() -> Pcount3DDemo_initTask() -> Pcount3DDemo_CLIInit() -> CLI_open() -> CLI_MMWaveExtensionInit() -> CLI_task()  

- CLI command 실행  
  Pcount3DDemo_CLISensorStop() ... -> Pcount3DDemo_CLISensorStart() -> CLI_getMMWaveExtensionConfig() -> CLI_getMMWaveExtensionOpenConfig -> ...  
<br>  

### CLI를 위해 필요한 파일  
- pcount3D_cli.c  
  people counting 용 command table entry 및 함수  

- cli_mmwave.c  
  people counting 프로젝트 이외에 다른 프로젝트에서도 사용되는 Extension command table entry 및 함수  
  (해당 파일이 include 되진 않고 extern으로 정의된 함수들만 불러들여짐)  

  (해당 파일의 함수를 추가로 사용하기 위해서  
  cli_open()에서 CLI_MMWaveExtensionInit(), CLI_getMMWaveExtensionConfig()을  
  cli_open에서 CLI_getMMWaveExtensionOpenConfig()을  
  cli_task()에서 CLI_MMWaveExtensionHandler()을 실행한다.)  

- cli.c / cli.h / cli_internal.h  
  cli task 및 structure 정의  

- tracker_utils.c / tracker_utils.h  
  tracking 용 함수  

- mmwdemo_adcconfig.c / mmwdemo_adcconfig.h  
  adc Buffer와 관련된 함수 정의

- pcount3D_mss.h  
  structure 정의  

- pcount3D_config.h  
  structure 정의  

- mmwave.h / mmwavelink.h  
  structure 정의  


### CLI Init  
**ti/mmwave_sdk_03_05_00_04/packages/ti/utils/cli/cli.h**  
```c  
typedef struct CLI_Cfg_t
{
    // CLI Prompt string (if any to be displayed)
    char*               cliPrompt;

    // Optional banner string if any to be displayed on startup of the CLI
    char*               cliBanner;

    // UART Command Handle used by the CLI
    UART_Handle         cliUartHandle;

    /**
     * The CLI has an mmWave extension which can be enabled by this field.
     * The extension supports the well define mmWave link CLI command(s)
     * In order to use the extension the application should have initialized and setup the mmWave.
     */
    uint8_t             enableMMWaveExtension;

    // The SOC driver handle is used to acquire device part number
    SOC_Handle          socHandle;

    /**
     * The mmWave control handle which needs to be specified if
     * the mmWave extensions are being used. The CLI Utility works only
     * in the FULL configuration mode. If the handle is opened in
     * MINIMAL configuration mode the CLI mmWave extension will fail
     */
    MMWave_Handle       mmWaveHandle;

    // Task Priority: The CLI executes in the context of a task which executes with this priority
    uint8_t             taskPriority;

    // Flag which determines if the CLI Write should use the UART in polled or blocking mode.
    bool                usePolledMode;

    // Flag which determines if the CLI should override the platform string reported in @ref CLI_MMWaveVersion.
    bool                overridePlatform;

    // Optional platform string to be used in @ref CLI_MMWaveVersion
    char*               overridePlatformString;

    // This is the table which specifies the supported CLI commands
    CLI_CmdTableEntry   tableEntry[CLI_MAX_CMD];
}CLI_Cfg;
```
<br>  

**overhead_3d_people_count_6843_mss/mss/pcount3D_cli.c**  
```c  
void Pcount3DDemo_CLIInit (uint8_t taskPriority)
{
    CLI_Cfg     cliCfg;
    char        demoBanner[110];
    uint32_t    cnt;

    /* Create Demo Banner to be printed out by CLI */
    sprintf(&demoBanner[0], 
                    "***********************************\n" \
                    "IWR68xx Indoor people counting demo"  \
                    "***********************************\n"
            );

    /* Initialize the CLI configuration: */
    memset ((void *)&cliCfg, 0, sizeof(CLI_Cfg));

    /* Populate the CLI configuration: */
    cliCfg.cliPrompt                    = "mmwDemo:/>";
    cliCfg.cliBanner                    = demoBanner;
    cliCfg.cliUartHandle                = gMmwMssMCB.commandUartHandle;
    cliCfg.taskPriority                 = taskPriority;
    cliCfg.socHandle                    = gMmwMssMCB.socHandle;
    cliCfg.mmWaveHandle                 = gMmwMssMCB.ctrlHandle;
    cliCfg.enableMMWaveExtension        = 1U;
    cliCfg.usePolledMode                = true;
    cliCfg.overridePlatform             = false;
    cliCfg.overridePlatformString       = NULL;    
    
    cnt=0;
    cliCfg.tableEntry[cnt].cmd            = "sensorStart";
    cliCfg.tableEntry[cnt].helpString     = "[doReconfig(optional, default:enabled)]";
    cliCfg.tableEntry[cnt].cmdHandlerFxn  = Pcount3DDemo_CLISensorStart;
    cnt++;

    cliCfg.tableEntry[cnt].cmd            = "sensorStop";
    cliCfg.tableEntry[cnt].helpString     = "No arguments";
    cliCfg.tableEntry[cnt].cmdHandlerFxn  = Pcount3DDemo_CLISensorStop;
    cnt++;

    cliCfg.tableEntry[cnt].cmd            = "dynamicRACfarCfg";
    cliCfg.tableEntry[cnt].helpString     = "<leftSkipSize> <rightSkipSize> <leftSkipSizeAzimuth> <rightSkipSizeAngle> <searchWinSizeRange> <searchWinSizeAngle> <guardSizeRange> <guardSizeAngle> <threRange> <threAngle> <threSidelob> <enSecondPass>";
    cliCfg.tableEntry[cnt].cmdHandlerFxn  = Pcount3DDemo_CLIDynRACfarCfg;
    cnt++;

    cliCfg.tableEntry[cnt].cmd            = "staticRACfarCfg";
    cliCfg.tableEntry[cnt].helpString     = "<leftSkipSize> <rightSkipSize> <leftSkipSizeAzimuth> <rightSkipSizeAngle> <searchWinSizeRange> <searchWinSizeAngle> <guardSizeRange> <guardSizeAngle> <threRange> <threAngle> <threSidelob> <enSecondPass>";
    cliCfg.tableEntry[cnt].cmdHandlerFxn  = Pcount3DDemo_CLIStaticRACfarCfg;
    cnt++;

    cliCfg.tableEntry[cnt].cmd            = "dynamicRangeAngleCfg";
    cliCfg.tableEntry[cnt].helpString     = "<subFrameIdx> <searchStep> <mvdr_alpha> <detectionMethod> <dopplerEstMethod>";
    cliCfg.tableEntry[cnt].cmdHandlerFxn  = Pcount3DDemo_CLIDynRngAngleCfg;
    cnt++;

    cliCfg.tableEntry[cnt].cmd            = "dynamic2DAngleCfg";
    cliCfg.tableEntry[cnt].helpString     = "<subFrameIdx> <elevSearchStep> <mvdr_alpha> <maxNpeak2Search> <peakExpSamples> <elevOnly> <sideLobThr> <peakExpRelThr> <peakExpSNRThr>";
    cliCfg.tableEntry[cnt].cmdHandlerFxn  = Pcount3DDemo_CLIDynAngleEstCfg;
    cnt++;

    cliCfg.tableEntry[cnt].cmd            = "dopplerCfarCfg";
    cliCfg.tableEntry[cnt].helpString     = "<subFrameIdx> <discardLeft> <discardRight> <guardWinSize> <refWinSize> <threshold> ";
    cliCfg.tableEntry[cnt].cmdHandlerFxn  = Pcount3DDemo_CLIDopplerCFARCfg;
    cnt++;

    cliCfg.tableEntry[cnt].cmd            = "staticRangeAngleCfg";
    cliCfg.tableEntry[cnt].helpString     = "<subFrameIdx> <staticProcEnabled> <staticAzimStepDeciFactor> <staticElevStepDeciFactor>";
    cliCfg.tableEntry[cnt].cmdHandlerFxn  = Pcount3DDemo_CLIStaticRngAngleCfg;
    cnt++;

    cliCfg.tableEntry[cnt].cmd            = "fineMotionCfg";
    cliCfg.tableEntry[cnt].helpString     = "<subFrameIdx> <fineMotionProcEnabled> <fineMotionObservationTime> <fineMotionProcCycle> <fineMotionDopplerThrIdx>";
    cliCfg.tableEntry[cnt].cmdHandlerFxn  = Pcount3DDemo_CLIFineMotionCfg;
    cnt++;

    cliCfg.tableEntry[cnt].cmd            = "fovCfg";
    cliCfg.tableEntry[cnt].helpString     = "<subFrameIdx> <azimFoV> <elevFoV> ";
    cliCfg.tableEntry[cnt].cmdHandlerFxn  = Pcount3DDemo_CLIAntAngleFoV;
    cnt++;

    cliCfg.tableEntry[cnt].cmd            = "antGeometry0";
    cliCfg.tableEntry[cnt].helpString     = "<elem0> <elem1> <elem2> <elem3> <elem4> <elem5> <elem6> <elem7> <elem8>  <elem9> <elem10> <elem11> <elem12> ";
    cliCfg.tableEntry[cnt].cmdHandlerFxn  = Pcount3DDemo_CLIBoardAntGeometry0;
    cnt++;

    cliCfg.tableEntry[cnt].cmd            = "antGeometry1";
    cliCfg.tableEntry[cnt].helpString     = "<elem0> <elem1> <elem2> <elem3> <elem4> <elem5> <elem6> <elem7> <elem8>  <elem9> <elem10> <elem11> <elem12> ";
    cliCfg.tableEntry[cnt].cmdHandlerFxn  = Pcount3DDemo_CLIBoardAntGeometry1;
    cnt++;

    cliCfg.tableEntry[cnt].cmd            = "antPhaseRot";
    cliCfg.tableEntry[cnt].helpString     = "<elem0> <elem1> <elem2> <elem3> <elem4> <elem5> <elem6> <elem7> <elem8>  <elem9> <elem10> <elem11> <elem12> ";
    cliCfg.tableEntry[cnt].cmdHandlerFxn  = Pcount3DDemo_CLIBoardAntPhaseRot;
    cnt++;

    cliCfg.tableEntry[cnt].cmd            = "adcbufCfg";
    cliCfg.tableEntry[cnt].helpString     = "<subFrameIdx> <adcOutputFmt> <SampleSwap> <ChanInterleave> <ChirpThreshold>";
    cliCfg.tableEntry[cnt].cmdHandlerFxn  = Pcount3DDemo_CLIADCBufCfg;
    cnt++;

    cliCfg.tableEntry[cnt].cmd            = "bpmCfg";
    cliCfg.tableEntry[cnt].helpString     = "<subFrameIdx> <enabled> <chirp0Idx> <chirp1Idx>";
    cliCfg.tableEntry[cnt].cmdHandlerFxn  = Pcount3DDemo_CLIBpmCfg;
    cnt++;

    cliCfg.tableEntry[cnt].cmd            = "compRangeBiasAndRxChanPhase";
    cliCfg.tableEntry[cnt].helpString     = "<rangeBias> <Re00> <Im00> <Re01> <Im01> <Re02> <Im02> <Re03> <Im03> <Re10> <Im10> <Re11> <Im11> <Re12> <Im12> <Re13> <Im13>  <Re20> <Im20> <Re21> <Im21> <Re22> <Im22> <Re23> <Im23> ";
    cliCfg.tableEntry[cnt].cmdHandlerFxn  = Pcount3DDemo_CLICompRangeBiasAndRxChanPhaseCfg;
    cnt++;

    cliCfg.tableEntry[cnt].cmd            = "resetDevice";
    cliCfg.tableEntry[cnt].helpString     = "No arguments";
    cliCfg.tableEntry[cnt].cmdHandlerFxn  = Pcount3DDemo_CLIResetDevice;
    cnt++;

    cliCfg.tableEntry[cnt].cmd            = "trackingCfg";
    cliCfg.tableEntry[cnt].helpString     = "<enable> <paramSet> <numPoints> <numTracks> <maxDoppler> <maxDopplerResolution> <framePeriod> [boresightFilteringFlag - Default is enabled]";
    cliCfg.tableEntry[cnt].cmdHandlerFxn  = MmwDemo_CLITrackingCfg;
    cnt++;

    cliCfg.tableEntry[cnt].cmd             = "staticBoundaryBox";
    cliCfg.tableEntry[cnt].helpString      = "<X min> <X Max> <Y min> <Y max> <Z min> <Z max>";
    cliCfg.tableEntry[cnt].cmdHandlerFxn   = MmwDemo_CLIStaticBoundaryBoxCfg;
    cnt++;

    cliCfg.tableEntry[cnt].cmd             = "boundaryBox";
    cliCfg.tableEntry[cnt].helpString      = "<X min> <X Max> <Y min> <Y max> <Z min> <Z max>";
    cliCfg.tableEntry[cnt].cmdHandlerFxn   = MmwDemo_CLIBoundaryBoxCfg;
    cnt++;

    cliCfg.tableEntry[cnt].cmd             = "sensorPosition";
    cliCfg.tableEntry[cnt].helpString      = "<Z - Height> <Azimuth Tilt> <Elevation Tilt>";
    cliCfg.tableEntry[cnt].cmdHandlerFxn   = MmwDemo_CLISensorPositionCfg;
    cnt++;
    cliCfg.tableEntry[cnt].cmd             = "gatingParam";// PC: 4 gating volume, Limits are set to 3m in length, 2m in width, 0 no limit in doppler
    cliCfg.tableEntry[cnt].helpString      = "<gating volume> <length> <width> <doppler>";
    cliCfg.tableEntry[cnt].cmdHandlerFxn   = MmwDemo_CLIGatingParamCfg;
    cnt++;
    
    cliCfg.tableEntry[cnt].cmd             = "stateParam";// PC: 10 frames to activate, 5 to forget, 10 active to free, 1000 static to free, 5 exit to free, 6000 sleep to free
    cliCfg.tableEntry[cnt].helpString      = "<det2act> <det2free> <act2free> <stat2free> <exit2free> <sleep2free>";//det2act, det2free, act2free, stat2free, exit2free, sleep2free
    cliCfg.tableEntry[cnt].cmdHandlerFxn   = MmwDemo_CLIStateParamCfg;
    cnt++;
    
    cliCfg.tableEntry[cnt].cmd             = "allocationParam";// PC: 250 SNR, 0.1 minimal velocity, 5 points, 1m in distance, 2m/s in velocity
    cliCfg.tableEntry[cnt].helpString      = "<SNRs> <minimal velocity> <points> <in distance> <in velocity>";
    cliCfg.tableEntry[cnt].cmdHandlerFxn   = MmwDemo_CLIAllocationParamCfg;
    cnt++;

    cliCfg.tableEntry[cnt].cmd             = "maxAcceleration";
    cliCfg.tableEntry[cnt].helpString      = "<max X acc.> <max Y acc.> <max Z acc.>";
    cliCfg.tableEntry[cnt].cmdHandlerFxn   = MmwDemoCLIMaxAccelerationParamCfg;
    cnt++;

    cliCfg.tableEntry[cnt].cmd             = "presenceBoundaryBox";
    cliCfg.tableEntry[cnt].helpString      = "<X min> <X Max> <Y min> <Y max> <Z min> <Z max>";
    cliCfg.tableEntry[cnt].cmdHandlerFxn   = MmwDemo_CLIPresenceParamCfg;
    cnt++;

    /* Open the CLI: */
    if (CLI_open (&cliCfg) < 0)
    {
        System_printf ("Error: Unable to open the CLI\n");
        return;
    }
    System_printf ("Debug: CLI is operational\n");
    return;
}
```
<br>  

### CLI Open  
**ti/mmwave_sdk_03_05_00_04/packages/ti/utils/cli/include/cli_internal.h**  
```c  
typedef struct CLI_MCB_t
{
    // Configuration which was used to configure the CLI module
    CLI_Cfg         cfg;

    // This is the number of CLI commands which have been added to the module
    uint32_t        numCLICommands;

    // CLI Task Handle:
    Task_Handle     cliTaskHandle;
}CLI_MCB;

/* CLI Global Variables */
extern CLI_MCB gCLI;
```
<br>  

**ti/mmwave_sdk_03_05_00_04/packages/ti/utils/cli/src/cli.c**  
```c  
int32_t CLI_open (CLI_Cfg* ptrCLICfg)
{
    Task_Params     taskParams;
    uint32_t        index;

    /* Sanity Check: Validate the arguments */
    if (ptrCLICfg == NULL)
        return -1;

    /* Initialize the CLI MCB: */
    memset ((void*)&gCLI, 0, sizeof(CLI_MCB));

    /* Copy over the configuration: */
    memcpy ((void *)&gCLI.cfg, (void *)ptrCLICfg, sizeof(CLI_Cfg));

    /* Cycle through and determine the number of supported CLI commands: */
    for (index = 0; index < CLI_MAX_CMD; index++)
    {
        /* Do we have a valid entry? */
        if (gCLI.cfg.tableEntry[index].cmd == NULL)
        {
            /* NO: This is the last entry */
            break;
        }
        else
        {
            /* YES: Increment the number of CLI commands */
            gCLI.numCLICommands = gCLI.numCLICommands + 1;
        }
    }

    /* Is the mmWave Extension enabled? */
    if (gCLI.cfg.enableMMWaveExtension == 1U)
    {
        /* YES: Initialize the CLI Extension: */
        if (CLI_MMWaveExtensionInit (ptrCLICfg) < 0)
            return -1;
    }

    /* Do we have a CLI Prompt specified?  */
    if (gCLI.cfg.cliPrompt == NULL)
        gCLI.cfg.cliPrompt = "CLI:/>";

    /* The CLI provides a help command by default:
     * - Since we are adding this at the end of the table; a user of this module can also
     *   override this to provide its own implementation. */
    gCLI.cfg.tableEntry[gCLI.numCLICommands].cmd           = "help";
    gCLI.cfg.tableEntry[gCLI.numCLICommands].helpString    = NULL;
    gCLI.cfg.tableEntry[gCLI.numCLICommands].cmdHandlerFxn = CLI_help;

    /* Increment the number of CLI commands: */
    gCLI.numCLICommands++;

    /* Initialize the task parameters and launch the CLI Task: */
    Task_Params_init(&taskParams);
    taskParams.priority  = gCLI.cfg.taskPriority;
    taskParams.stackSize = 4*1024;
    gCLI.cliTaskHandle = Task_create(CLI_task, &taskParams, NULL);
    return 0;
}
```
<br>  

### CLI Task  
**ti/mmwave_sdk_03_05_00_04/packages/ti/utils/cli/src/cli.c**  
```c
static void CLI_task(UArg arg0, UArg arg1)
{
    uint8_t                 cmdString[256];
    char*                   tokenizedArgs[CLI_MAX_ARGS];
    char*                   ptrCLICommand;
    char                    delimitter[] = " \r\n";
    uint32_t                argIndex;
    CLI_CmdTableEntry*      ptrCLICommandEntry;
    int32_t                 cliStatus;
    uint32_t                index;

    /* Do we have a banner to be displayed? */
    if (gCLI.cfg.cliBanner != NULL)
    {
        /* YES: Display the banner */
        CLI_write (gCLI.cfg.cliBanner);
    }

    /* Loop around forever: */
    while (1)
    {
        /* Demo Prompt: */
        CLI_write (gCLI.cfg.cliPrompt);

        /* Reset the command string: */
        memset ((void *)&cmdString[0], 0, sizeof(cmdString));

        /* Read the command message from the UART: */
        UART_read (gCLI.cfg.cliUartHandle, &cmdString[0], (sizeof(cmdString) - 1));

        /* Reset all the tokenized arguments: */
        memset ((void *)&tokenizedArgs, 0, sizeof(tokenizedArgs));
        argIndex      = 0;
        ptrCLICommand = (char*)&cmdString[0];

        /* comment lines found - ignore the whole line*/
        if (cmdString[0]=='%') {
            CLI_write ("Skipped\n");
            continue;
        }

        /* Set the CLI status: */
        cliStatus = -1;

        /* The command has been entered we now tokenize the command message */
        while (1)
        {
            /* Tokenize the arguments: */
            tokenizedArgs[argIndex] = strtok(ptrCLICommand, delimitter);
            if (tokenizedArgs[argIndex] == NULL)
                break;

            /* Increment the argument index: */
            argIndex++;
            if (argIndex >= CLI_MAX_ARGS)
                break;

            /* Reset the command string */
            ptrCLICommand = NULL;
        }

        /* Were we able to tokenize the CLI command? */
        if (argIndex == 0)
            continue;

        /* Cycle through all the registered CLI commands: */
        for (index = 0; index < gCLI.numCLICommands; index++)
        {
            ptrCLICommandEntry = &gCLI.cfg.tableEntry[index];

            /* Do we have a match? */
            if (strcmp(ptrCLICommandEntry->cmd, tokenizedArgs[0]) == 0)
            {
                /* YES: Pass this to the CLI registered function */
                cliStatus = ptrCLICommandEntry->cmdHandlerFxn (argIndex, tokenizedArgs);
                if (cliStatus == 0)
                {
                    CLI_write ("Done\n");
                }
                else
                {
                    CLI_write ("Error %d\n", cliStatus);
                }
                break;
            }
        }

        /* Did we get a matching CLI command? */
        if (index == gCLI.numCLICommands)
        {
            /* NO matching command found. Is the mmWave extension enabled? */
            if (gCLI.cfg.enableMMWaveExtension == 1U)
            {
                /* Yes: Pass this to the mmWave extension handler */
                cliStatus = CLI_MMWaveExtensionHandler (argIndex, tokenizedArgs);
            }

            /* Was the CLI command found? */
            if (cliStatus == -1)
            {
                /* No: The command was still not found */
                CLI_write ("'%s' is not recognized as a CLI command\n", tokenizedArgs[0]);
            }
        }
    }
}
```
