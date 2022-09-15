# utl-enhanced-common-meta-data-model-to-reduce-CDSIC-mapping-efforts
     Create a common meta data model to reduce CDISC mapping efforts enhanced version
    /************************************************************************************************************************************************/
    /*                                                                                                                                              */
    /*  %let pgm=cdd_010mta;                                                                                                                        */
    /*                                                                                                                                              */
    /*  %let pgm=utl-enhanced-common-meta-data-model-to-reduce-CDSIC-mapping-efforts;                                                               */
    /*                                                                                                                                              */
    /*  Program location: d:/cdm/oto/cdm_010mta.sas                                                                                                 */
    /*                                                                                                                                              */
    /*  %let purpose =Create a common meta data model to reduce CDISC mapping efforts enhanced version;                                             */
    /*                                                                                                                                              */
    /*  To use this code to build the directory structure below but you have to populate it with all your, raw, sdtm, adam, specs and programs.     */
    /*  I have a provided cmoplete reproducible example, just replace with your studies, however each stdy will need a three character unique ID.   */
    /*                                                                                                                                              */
    /*  Only output so needed ( you can use this directly and not run any code)                                                                     */
    /*                                                                                                                                              */
    /*    d:/cdm/cdm_010mta130pgm.sas7bdat  (common met data model)                                                                                 */
    /*                                                                                                                                              */
    /*  Suppose you want all the distinct AETERMS in the raw data are represented in the SDTM and  ADAM datasets                                    */
    /*                                                                                                                                              */
    /*  proc sql;                                                                                                                                   */
    /*    select                                                                                                                                    */
    /*      *                                                                                                                                       */
    /*    from                                                                                                                                      */
    /*      "d:/cdm/cdm_010MTA130.sas7bdat"                                                                                                         */
    /*    where                                                                                                                                     */
    /*           variable contains "AETERM"                                                                                                         */
    /*       and question in ("ADM_DISTINCT", "SDM_DISTINCT", "RAW_DISTINCT")                                                                       */
    /*                                                                                                                                              */
    /*  ;quit;                                                                                                                                      */
    /*                                                                                                                                              */
    /*  SO AT LEAST WE HAVE THE DISTINCT VALUES IN THE RAW ALSO IN THE SDTM AND ADAM DATASETS                                                       */
    /*                                                                                                                                              */
    /*                                                                                                                                              */
    /*  SAS CRF                                                                                                                                     */
    /*  DDT SDM                                                                                                                                     */
    /*  DMP -                                          Table SAS                                                                                    */
    /*  Source for  Study       Clinical Dataset       Dataset/CRF        Question ie Label, Number                                                 */
    /*  meta data   Identifier  CRT/SDTM/ADAM          Module..           of Obs and Max Min         Answer                                         */
    /*  ---------------------------------------------------------------------------------------------------                                         */
    /*  CDM_SCHEMA  C01ADM      C01ADM_ADAE            AETERM             ADM_DISTINCT               36                                             */
    /*  CDM_SCHEMA  C01RAW      C01RAW_RAWADVERSE      AETERM             RAW_DISTINCT               36                                             */
    /*  CDM_SCHEMA  C01SDM      C01SDM_AE              AETERM             SDM_DISTINCT               36                                             */
    /*  CDM_SCHEMA  C02ADM      C02ADM_ADAE            AETERM             ADM_DISTINCT               36                                             */
    /*  CDM_SCHEMA  C02RAW      C02RAW_RAWADVERSE      AETERM             RAW_DISTINCT               36                                             */
    /*  CDM_SCHEMA  C02SDM      C02SDM_AE              AETERM             SDM_DISTINCT               36                                             */
    /*                                                                                                                                              */
    /*                                                                                                                                              */
    /*  You can aslo query programs for the variable AETERM                                                                                         */
    /*                                                                                                                                              */
    /*  proc sql;                                                                                                                                   */
    /*    select                                                                                                                                    */
    /*      *                                                                                                                                       */
    /*    from                                                                                                                                      */
    /*      "d:/cdm/cdm_010MTA130.sas7bdat"                                                                                                         */
    /*    where                                                                                                                                     */
    /*            upcase(answer)   contains "AESTDTC"                                                                                               */
    /*       and  question contains '_PROGRAM'                                                                                                      */
    /*                                                                                                                                              */
    /*  ;quit;                                                                                                                                      */
    /*                                                                                                                                              */
    /*  IT LOOKS LIKE  AEDY AND AEDT ARE CREATED USING THE MACROS BELOW                                                                             */
    /*                                                                                                                                              */
    /*                                                                                                                                              */
    /*  SAS CRF                                                                                                                                     */
    /*  DDT SDM                                                                                                                                     */
    /*  DMP -                                                                                                                                       */
    /*  Source for  Study       Clinical DatasetQuestion ie Label, Number                                                                           */
    /*  meta data   Identifier  CRT/SDTM/ADAM   of Obs and Max Min         Answer Staements.                                                        */
    /*  -----------------------------------------------------------------------------------------------------------------------------------         */
    /*  CDM_SCHEMA  C01ADM      C01ADM_AE       ADM_PROGRAM                00033         %cln_160dtc2dt(aestdtc, prefix=ast, refdt=trtsdt);         */
    /*  CDM_SCHEMA  C01SDM      C01SDM_AE       SDM_PROGRAM                00028         %cln_110mkesdtmdy(refdate=rfstdtc,date=aestdtc);           */
    /*  CDM_SCHEMA  C01SDM      C01SDM_AE       SDM_PROGRAM                00037         by studyid usubjid aedecod aestdtc aeendtc;                */
    /*  CDM_SCHEMA  C02ADM      C02ADM_AE       ADM_PROGRAM                00033         %cln_160dtc2dt(aestdtc, prefix=ast, refdt=trtsdt);         */
    /*  CDM_SCHEMA  C02SDM      C02SDM_AE       SDM_PROGRAM                00028         %cln_110mkesdtmdy(refdate=rfstdtc,date=aestdtc);           */
    /*                                                                                               of Adverse Event                               */
    /*                                                                                                                                              */
    /*  IF WE WANT TO KNOW THE LOCATIO N OF THE PROGRAMS                                                                                            */
    /*                                                                                                                                              */
    /*  proc sql;                                                                                                                                   */
    /*    select                                                                                                                                    */
    /*      *                                                                                                                                       */
    /*    from                                                                                                                                      */
    /*      "d:/cdm/cdm_010MTA130.sas7bdat"                                                                                                         */
    /*    where                                                                                                                                     */
    /*            table   contains "AE"                                                                                                             */
    /*       and  question contains '_PROGRAM_PATH'                                                                                                 */
    /*                                                                                                                                              */
    /*  ;quit;                                                                                                                                      */
    /*                                                                                                                                              */
    /*                                                                                                                                              */
    /*  SAS CRF                                                                                                                                     */
    /*  DDT SDM                                                                                                                                     */
    /*  DMP -                                          Table SAS                                                                                    */
    /*  Source for  Study       Clinical Dataset       Dataset/CRF        Question ie Label, Number                                                 */
    /*  meta data   Identifier  CRT/SDTM/ADAM          Module..           of Obs and Max Min        location of library..                           */
    /*  -----------------------------------------------------------------------------------------------------------------------                     */
    /*  CDM_SCHEMA  C01ADM      C01ADM_AE              ***TABLE_LEVEL***  ADM_PROGRAM_PATH           d:\cdm\sas\c01adm_ae.sas                       */
    /*  CDM_SCHEMA  C01SDM      C01SDM_AE              ***TABLE_LEVEL***  SDM_PROGRAM_PATH           d:\cdm\sas\c01sdm_ae.sas                       */
    /*  CDM_SCHEMA  C02ADM      C02ADM_AE              ***TABLE_LEVEL***  ADM_PROGRAM_PATH           d:\cdm\sas\c02adm_ae.sas                       */
    /*  CDM_SCHEMA  C02SDM      C02SDM_AE              ***TABLE_LEVEL***  SDM_PROGRAM_PATH           d:\cdm\sas\c02sdm_ae.sas                       */
    /*                                                                                                                                              */
    /*  _ __ ___  __ _  __| |_ __ ___   ___                                                                                                         */
    /* | `__/ _ \/ _` |/ _` | `_ ` _ \ / _ \                                                                                                        */
    /* | | |  __/ (_| | (_| | | | | | |  __/                                                                                                        */
    /* |_|  \___|\__,_|\__,_|_| |_| |_|\___|                                                                                                        */
    /*                                                                                                                                              */
    /*  1. Download all the externals                                                                                                               */
    /*     Create a folder for this project, I suggest d:/cdm or any other drive:/cdm                                                               */
    /*                                                                                                                                              */
    /*  2. Download clj.zip and extract the external inputs into the folder you created in step 1.                                                  */
    /*                                                                                                                                              */
    /*  3. Copy program c:/cdm/sas/cdm_010mta.sas into your editor (or use this readme or .sas version)                                             */
    /*     Edit the root macro variable                                                                                                             */
    /*                                                                                                                                              */
    /*  4. You should be able to run the entire program, but I suggest you highlight and run blocks of code                                         */
    /*                                                                                                                                              */
    /*  I hope I got all the dependncies on d:/cdm                                                                                                  */
    /*                                                                                                                                              */
    /*  github                                                                                                                                      */
    /*  https://tinyurl.com/2p8jjrdt                                                                                                                */
    /*  https://github.com/rogerjdeangelis/utl-reduce-your-sdtm-and-adam-mapping-effort-using-a-common-meta-data-model                              */
    /*                                                                                                                                              */
    /*  Related to                                                                                                                                  */
    /*  https://tinyurl.com/2zps56xc                                                                                                                */
    /*  https://github.com/rogerjdeangelis/utl-end-to-end-cdisc-SDTM-ADaM-processing                                                                */
    /*                                                                                                                                              */
    /*                                                                                                                                              */
    /************************************************************************************************************************************************/
    /*                                                                                                                                              */
    /*  Description                                                                                                                                 */
    /*                                                                                                                                              */
    /*  Common Meta Data Model(CMDM) mines all previous mapping work to locate reusable mapping code, create excel meta sheeta and, make define.xml */
    /*                                                                                                                                              */
    /*  It is like the define.xml on steroids                                                                                                       */
    /*                                                                                                                                              */
    /*  Can create the excel meta workbooks and define.xml directly from the CMDM.                                                                  */
    /*                                                                                                                                              */
    /*  This is a simplified CMDM, the full model can do things like identify unmapped raw variables. Also has all the promary keys for all tables. */
    /*                                                                                                                                              */
    /*  Full CMDM has Dimension tables with CRF, SAP, PROTOCOL, SDTM IG, SDTM Guidance, SDTM contrlled terminology                                  */
    /*                                                                                                                                              */
    /*  I have some AI to do thinks like capture mappings from annotated CRFs                                                                       */
    /*                                                                                                                                              */
    /************************************************************************************************************************************************/
    /*       _                           _                 _                                                                                        */
    /*    __| | ___ _ __   ___ _ __   __| | ___ _ __   ___(_) ___  ___                                                                              */
    /*   / _` |/ _ \ `_ \ / _ \ `_ \ / _` |/ _ \ `_ \ / __| |/ _ \/ __|                                                                             */
    /*  | (_| |  __/ |_) |  __/ | | | (_| |  __/ | | | (__| |  __/\__ \                                                                             */
    /*   \__,_|\___| .__/ \___|_| |_|\__,_|\___|_| |_|\___|_|\___||___/                                                                             */
    /*             |_|                                                                                                                              */
    /*                                                                                                                                              */
    /*   Win 10 pro workstation 64bit                                                                                                               */
    /*   SAS 9.4 M7 64bit                                                                                                                           */
    /*                                                                                                                                              */
    /*   EXTERNAL MACROS ALL OF THESE ARE IN AUTOCALL FOLDER D:/CMD/OTO                                                                             */
    /*   ===============================================================                                                                            */
    /*                                                                                                                                              */
    /*   utlnopts - turn options off to minimize the log                                                                                            */
    /*   utlopts  - turn options on for debugginf                                                                                                   */
    /*                                                                                                                                              */
    /*   utl_optlenpos - seet valable length to the max observed                                                                                    */
    /*                                                                                                                                              */
    /*   utl_dirlst    - create sas data set will all files in a folder                                                                             */
    /*                                                                                                                                              */
    /*   odsoff      - turn ods off                                                                                                                 */
    /*   odson       - turn ods on                                                                                                                  */
    /*                                                                                                                                              */
    /*   array                                                                                                                                      */
    /*   do_over                                                                                                                                    */
    /*                                                                                                                                              */
    /*   stop_submission - gently stops SAS processing                                                                                              */
    /*                                                                                                                                              */
    /*                                                                                                                                              */
    /*   INTERNAL MACROS ALL OF THESE ARE IN AUTOCALL FOLDER D:/CLS/OTO                                                                             */
    /*   ===============================================================                                                                            */
    /*                                                                                                                                              */
    /*    NONE                                                                                                                                      */
    /*                                                                                                                                              */
    /*   VERSION FOLDER                                                                                                                             */
    /*                                                                                                                                              */
    /*                                                                                                                                              */
    /************************************************************************************************************************************************/
    /*                                                                                                                                              */
    /*   _                   _                          _                                                         _       _       _                 */
    /*  (_)_ __  _ __  _   _| |_   _ __  _ __ _____   _(_) ___  _   _ ___    _ __ ___   __ _ _ __  _ __   ___  __| |   __| | __ _| |_ __ _          */
    /*  | | `_ \| `_ \| | | | __| | `_ \| `__/ _ \ \ / / |/ _ \| | | / __|  | `_ ` _ \ / _` | `_ \| `_ \ / _ \/ _` |  / _` |/ _` | __/ _` |         */
    /*  | | | | | |_) | |_| | |_  | |_) | | |  __/\ V /| | (_) | |_| \__ \  | | | | | | (_| | |_) | |_) |  __/ (_| | | (_| | (_| | || (_| |         */
    /*  |_|_| |_| .__/ \__,_|\__| | .__/|_|  \___| \_/ |_|\___/ \__,_|___/  |_| |_| |_|\__,_| .__/| .__/ \___|\__,_|  \__,_|\__,_|\__\__,_|         */
    /*          |_|               |_|                                                       |_|   |_|                                               */
    /*             _                        _   _                   _                                                                               */
    /*    _____  _| |_ ___ _ __ _ __   __ _| | (_)_ __  _ __  _   _| |_ ___                                             _               _           */
    /*   / _ \ \/ / __/ _ \ `__| `_ \ / _` | | | | `_ \| `_ \| | | | __/ __|                                 ___  _   _| |_ _ __  _   _| |_ ___     */
    /*  |  __/>  <| ||  __/ |  | | | | (_| | | | | | | | |_) | |_| | |_\__ \                                / _ \| | | | __| `_ \| | | | __/ __|    */
    /*   \___/_/\_\\__\___|_|  |_| |_|\__,_|_| |_|_| |_| .__/ \__,_|\__|___/                               | (_) | |_| | |_| |_) | |_| | |_\__ \    */
    /*                                                 |_|                                                  \___/ \__,_|\__| .__/ \__,_|\__|___/    */
    /*                                                                                                                     |_|                      */
    /*  d:/cdmlx                                                                                                                                    */
    /*  |                                                                                                                                           */
    /*  +---fmt                                                                                                                                     */
    /*  |       c01raw_rawfmt.sas7bcat * C01  COVID Adult Vacine Study Formats                                                                      */
    /*  |       c02raw_rawfmt.sas7bcat * C02  COVID Pedriatic Study    Formats           D:/cdm/cdm_010MTA130 (Central DMDM fact table)             */
    /*  |                                                                                                                                           */
    /*  +---oto                        * Tools SAS autocall folder                        Variable   Label                                          */
    /*  |       array.sas                                                                                                                           */
    /*  |       arraydelete.sas                                                           SRC        SAS CRF DDT SDM DMP - Source for meta data     */
    /*  |       .......                                                                   STUDY      Study Identifier                               */
    /*  |       utl_ymrlan100.sas                                                         TABLE      Clinical Dataset CRT/SDTM/ADAM                 */
    /*  |       xpy.sas                                                                   VARIABLE   Table SAS Dataset/CRF Module..                 */
    /*  |                                                                                 QUESTION   Question ie Label, Number of Obs and Max Min   */
    /*  +---sas                        * Past programs to Map SDTMS                       ANSWER     Answer ie number of obs, location of library.. */
    /*  |      c01sdm_Ae.sas           *  C01  COVID Adult Vacine Study Formats                                 /                                   */
    /*  |      c01sdm_Dm.sas                                                                                   /                                    */
    /*  |      c01sdm_Ex.sas                                                                                  /                                     */
    /*  |      c01sdm_Lb.sas                                                                                 /                                      */
    /*  |      c01sdm_Xp.sas                                                                                /                                       */
    /*  |                                                                                                  /                                        */
    /*  |      c02sdm_Ae.sas                                                                              /                                         */
    /*  |      c02sdm_Dm.sas                                                                             /                                          */
    /*  |      c02sdm_Ex.sas                                                                            /                                           */
    /*  |      c02sdm_Lb.sas                                                                           /                                            */
    /*  |      c02sdm_Xp.sas                                                                          /                                             */
    /*  +---sd1                                                                                      /                                              */
    /*         c01raw_rawadverse.sas7bdat      * Raw Client COVID Adult Study                       /                                               */
    /*         c01raw_rawdemographic.sas7bdat                                                      /                                                */
    /*         c01raw_rawdosing.sas7bdat                                                          /                                                 */
    /*         c01raw_rawlabs.sas7bdat                                                  ______   /                                                  */
    /*         c01raw_rawpain.sas7bdat                                                 /       \                                                    */
    /*                                                                         ------>|  ROOT   |                                                   */
    /*         c02raw_rawadverse.sas7bdat      * Raw Client COVID Child Study         |         |                                                   */
    /*         c02raw_rawdemographic.sas7bdat                                         | PROCES  |                                                   */
    /*         c02raw_rawdosing.sas7bdat                                               \______ /                                                    */
    /*         c02raw_rawlabs.sas7bdat                                                                                                              */
    /*         c02raw_rawpain.sas7bdat                                                                                                              */
    /*                                                                                                                                              */
    /*         c01sdm_ae.sas7bdat              * Previously mapped SDTMs                                                                            */
    /*         c01sdm_dm.sas7bdat                                                                                                                   */
    /*         c01sdm_ex.sas7bdat                                                                                                                   */
    /*         c01sdm_lb.sas7bdat                                                                                                                   */
    /*         c01sdm_xp.sas7bdat                                                                                                                   */
    /*                                                                                                                                              */
    /*         c02sdm_ae.sas7bdat                                                                                                                   */
    /*         c02sdm_dm.sas7bdat                                                                                                                   */
    /*         c02sdm_ex.sas7bdat                                                                                                                   */
    /*         c02sdm_lb.sas7bdat                                                                                                                   */
    /*         c02sdm_xp.sas7bdat                                                                                                                   */
    /*                                                                                                                                              */
    /*         c01adm_adae.sas7bdat            * Previously mapped ADaMs                                                                            */
    /*         c01adm_adef.sas7bdat                                                                                                                 */
    /*         c01adm_adsl.sas7bdat                                                                                                                 */
    /*         c01adm_adte.sas7bdat                                                                                                                 */
    /*         c02adm_adae.sas7bdat                                                                                                                 */
    /*         c02adm_adef.sas7bdat                                                                                                                 */
    /*         c02adm_adsl.sas7bdat                                                                                                                 */
    /*         c02adm_adte.sas7bdat                                                                                                                 */
    /*                                                                                                                                              */
    /*                                                                                                                                              */
    /************************************************************************************************************************************************/
    /*               _               _                                                                                                              */
    /*    ___  _   _| |_ _ __  _   _| |_                                                                                                            */
    /*   / _ \| | | | __| `_ \| | | | __|                                                                                                           */
    /*  | (_) | |_| | |_| |_) | |_| | |_                                                                                                            */
    /*   \___/ \__,_|\__| .__/ \__,_|\__|                                                                                                           */
    /*                                                              _              _       _                              _      _                  */
    /*    ___ ___  _ __ ___  _ __ ___   ___  _ __    _ __ ___   ___| |_ __ _    __| | __ _| |_ __ _   _ __ ___   ___   __| | ___| |                 */
    /*   / __/ _ \| `_ ` _ \| `_ ` _ \ / _ \| `_ \  | `_ ` _ \ / _ \ __/ _` |  / _` |/ _` | __/ _` | | `_ ` _ \ / _ \ / _` |/ _ \ |                 */
    /*  | (_| (_) | | | | | | | | | | | (_) | | | | | | | | | |  __/ || (_| | | (_| | (_| | || (_| | | | | | | | (_) | (_| |  __/ |                 */
    /*   \___\___/|_| |_| |_|_| |_| |_|\___/|_| |_| |_| |_| |_|\___|\__\__,_|  \__,_|\__,_|\__\__,_| |_| |_| |_|\___/ \__,_|\___|_|                 */
    /*                                                                                                                                              */
    /*                                                                                                                                              */
    /*   Two Tables (so far for the star/snowflake schema)                                                                                          */
    /*                                                                                                                                              */
    /*   d:/cdm/cdm_010mta120.sas7bdat     The meta data similar to OHDSI/OMOP normalized question/answer table.                                    */
    /*                                                                                                                                              */
    /*   Contents of d:/cdm/cdm_010mta130.sas7bdat (can easily be hundreds of thousands os records)                                                 */
    /*                                                                                                                                              */
    /*   Variables in Creation Order                                                                                                                */
    /*                                                                                                                                              */
    /*   #    Variable    Type     Len    Label                                                                                                     */
    /*                                                                                                                                              */
    /*   1    SRC         Char      16    SAS CRF DDT SDM DMP - Source for meta data                                                                */
    /*   2    STUDY       Char      32    Study Identifier                                                                                          */
    /*   3    TABLE       Char      32    Clinical Dataset CRT/SDTM/ADAM                                                                            */
    /*   4    VARIABLE    Char      32    Table SAS Dataset/CRF Module..                                                                            */
    /*   5    QUESTION    Char      32    Question ie Label, Number of Obs and Max Min                                                              */
    /*   6    ANSWER      Char    8192    Answer ie number of obs, location of library..                                                            */
    /*                            _                           _                                        _                                            */
    /*  ___  __ _ _ __ ___  _ __ | | ___    ___ _ __ ___   __| |_ __ ___    _ __ ___  ___ ___  _ __ __| |___                                        */
    /* / __|/ _` | `_ ` _ \| `_ \| |/ _ \  / __| `_ ` _ \ / _` | `_ ` _ \  | `__/ _ \/ __/ _ \| `__/ _` / __|                                       */
    /* \__ \ (_| | | | | | | |_) | |  __/ | (__| | | | | | (_| | | | | | | | | |  __/ (_| (_) | | | (_| \__ \                                       */
    /* |___/\__,_|_| |_| |_| .__/|_|\___|  \___|_| |_| |_|\__,_|_| |_| |_| |_|  \___|\___\___/|_|  \__,_|___/                                       */
    /*                     |_|                                                                                                                      */
    /*                                                                                                                                              */
    /*  MAXIMUM and MINIMUM AGE                                                                                                                     */
    /*  =======================                                                                                                                     */
    /*                                                                                                                                              */
    /*   SDTM AGE for C01 Study, COVID Adults Study and C02 Study, COVID Pediatric Study                                                            */
    /*                                                                                                                                              */
    /*    Obs       SRC        STUDY       TABLE      VARIABLE     QUESTION      ANSWER                                                             */
    /*                                                                                                                                              */
    /*   5260    cdm_SCHEMA    C01SDM    C01SDM_DM      AGE       SDM_MIN_MAX    28@70    C01 Study, COVID Adults Study                             */
    /*   5390    cdm_SCHEMA    C02SDM    C02SDM_DM      AGE       SDM_MIN_MAX     5@17    C02 Study, COVID Pediatric Study                          */
    /*                                                                                                                                              */
    /*                                                                                                                                              */
    /*  LAB TESTS                                                                                                                                   */
    /*  =========                                                                                                                                   */
    /*                                                                                                                                              */
    /*   All the SDTM Lab Tests in both Studies ( ALBUMIN=30 Lab Test then frequency, @ is delimiter)                                               */
    /*                                                                                                                                              */
    /*          SRC        STUDY       TABLE      VARIABLE     QUESTION                                                                             */
    /*                                                                                                                                              */
    /* 7217  cdm_SCHEMA    C02SDM    C02SDM_LB     LBTEST     SDM_FREQUENCIES                                                                       */
    /*                                                                                                                                              */
    /*  Obs                                                                ANSWER                                                                   */
    /*                                                                                                                                              */
    /* 7217  ALBUMIN=30@ALK. PHOS.=30@ALT (SGPT)=30@AST (SGOT)=30@DIRECT BILI=30@GGTP=30@HEMATOCRIT=30@HEMOGLOBIN=30@TOTAL BILI=30@TOTAL PROT=30    */
    /*                                                                                                                                              */
    /*    LBTEST        COUNT                                                                                                                       */
    /*                                                                                                                                              */
    /*    ALBUMIN         30                                                                                                                        */
    /*    ALK. PHOS.      30                                                                                                                        */
    /*    ALT (SGPT)      30                                                                                                                        */
    /*    AST (SGOT)      30                                                                                                                        */
    /*    DIRECT BILI     30                                                                                                                        */
    /*    GGTP            30                                                                                                                        */
    /*    HEMATOCRIT      30                                                                                                                        */
    /*    HEMOGLOBIN      30                                                                                                                        */
    /*    TOTAL BILI      30                                                                                                                        */
    /*    TOTAL PROT      30                                                                                                                        */
    /*                                                                                                                                              */
    /* MAPPING DOSE                                                                                                                                 */
    /* ============                                                                                                                                 */
    /*                                                                                                                                              */
    /*  Querying d:/cdm/cdm_010mta120sas for the mapping of SDTM variable EXDOSE                                                                    */
    /*                                                                                                                                              */
    /*                                                                      LINE_                                                                   */
    /*   Obs          SDTM_PROGRAM              SAS_STATEMENT              NUMBER                                                                   */
    /*                                                                                                                                              */
    /*   312    D:\cdm\SAS\C01SDM_EX.SAS        exdose = dailydose;          13                                                                     */
    /*                                                                                                                                              */
    /************************************************************************************************************************************************/
    /*                                                                                                                                              */
    /*   _ __  _ __ ___   ___ ___  ___ ___                                                                                                          */
    /*  | `_ \| `__/ _ \ / __/ _ \/ __/ __|                                                                                                         */
    /*  | |_) | | | (_) | (_|  __/\__ \__ \                                                                                                         */
    /*  | .__/|_|  \___/ \___\___||___/___/                                                                                                         */
    /*  |_|                                                                                                                                         */
    /*                                                                                                                                              */
    /*                                                                                                                                              */
    /*   1. Download 7zip file with all the external inputs from github                                                                             */
    /*      Unzip into your root directory. I used d:/cdm for the COVID studies                                                                     */
    /*                                                                                                                                              */
    /*   2. Clear all datasets in the work directory                                                                                                */
    /*      Clear macro arrays (this is especially useful f you are testing interactively and need to rerun)                                        */
    /*                                                                                                                                              */
    /*   INIT                                                                                                                                       */
    /*                                                                                                                                              */
    /*     1. Download 7zip file with all the external inputs from github                                                                           */
    /*        Unzip into your root directory. I used d:/cdm for the COVID studies                                                                   */
    /*                                                                                                                                              */
    /*     2. Clear all datasets in the work directory                                                                                              */
    /*        Clear macro arrays (this is especially useful f you are testing interactively and need to rerun)                                      */
    /*                                                                                                                                              */
    /*     3. Create attribute template for CMDM (these are the only variables that will be in the CMDM fact table                                  */
    /*                                                                                                                                              */
    /*         %let atr=%str(                                                                                                                       */
    /*         length                                                                                                                               */
    /*            Src           $16      /* SAS CRF DDT SDM DMP - Source for meta data */                                                           */
    /*            Study         $32                                                                                                                 */
    /*            Table         $32                                                                                                                 */
    /*            Variable      $32                                                                                                                 */
    /*            Question      $32     /* question see below for some of the questions */                                                          */
    /*            Answer        $8192;                                                                                                              */
    /*         label                                                                                                                                */
    /*            Src      = "SAS CRF DDT SDM DMP - Source for meta data"                                                                           */
    /*            Study    = "Study Identifier"                                                                                                     */
    /*            Table    = "Clinical Dataset CRT/SDTM/ADAM"                                                                                       */
    /*            Variable = "Table SAS Dataset/CRF Module.."                                                                                       */
    /*            Question = "Question ie Label, Number of Obs and Max Min"                                                                         */
    /*            Answer   = "Answer ie number of obs, location of library..";                                                                      */
    /*          keep                                                                                                                                */
    /*            Src                                                                                                                               */
    /*            Study                                                                                                                             */
    /*            Table                                                                                                                             */
    /*            Variable                                                                                                                          */
    /*            Question                                                                                                                          */
    /*            Answer;);                                                                                                                         */
    /*                                                                                                                                              */
    /*     4. The external folder d:/cdm/sd1 has all of the raw datasets, mapped sdtm datasets and the adam datasets                                 */
    /*        Create a a temp dataset with all of the datasets names                                                                                */
    /*                                                                                                                                              */
    /*     5. Assign a libname to the folder with all the external input table                                                                      */
    /*                                                                                                                                              */
    /*     6. Query the sas sas dictionary.vcolum to get type, length, labels and formats for all                                                   */
    /*        variables in all of the tables                                                                                                        */
    /*                                                                                                                                              */
    /*   MACRO ARRAYs                                                                                                                               */
    /*                                                                                                                                              */
    /*     7. Create macro array _jusTbls, with our 28 external tables in ur studies.                                                               */
    /*                                                                                                                                              */
    /*          _justbl1  ie C01SDM_AE                                                                                                              */
    /*          _justbl2                                                                                                                            */
    /*          _justbl3                                                                                                                            */
    /*          ..                                                                                                                                  */
    /*          _justbl28                                                                                                                           */
    /*                                                                                                                                              */
    /*          and                                                                                                                                 */
    /*          _justbln (which has the value 28)                                                                                                   */
    /*                                                                                                                                              */
    /*      8. Create these additional macro arrays                                                                                                 */
    /*                                                                                                                                              */
    /*          _tbl#      all tables                                                                                                               */
    /*          _var#      all vars                                                                                                                 */
    /*                                                                                                                                              */
    /*          _tblchr#   tables with char vars                                                                                                    */
    /*          _varchr#   char vars in tables                                                                                                      */
    /*                                                                                                                                              */
    /*          _tblnum#   tables with numeric vars                                                                                                 */
    /*          _varnum#   numeric vars in tables                                                                                                   */
    /*                                                                                                                                              */
    /*    POPULATE ATTRIBUTE TYPE, LENGTH ....                                                                                                      */
    /*                                                                                                                                              */
    /*      9. Populate attribute template with data from all tables and all variables for these questions                                          */
    /*                                                                                                                                              */
    /*           RAW_LABEL       SDM_LABEL      ADM_LABEL                                                                                           */
    /*           RAW_FORMAT      SDM_FORMAT     ADM_FORMAT                                                                                          */
    /*           RAW_INFORMAT    SDM_INFORMAT   ADM_INFORMAT                                                                                        */
    /*           RAW_TYPE        SDM_TYPE       ADM_TYPE                                                                                            */
    /*           RAW_LENGTH      SDM_LENGTH     ADM_LENGTH                                                                                          */
    /*                                                                                                                                              */
    /*      10.  Populate N-Levels N-MissLevels N-NonMissLevels for all variables                                                                   */
    /*                                                                                                                                              */
    /*      11.  Populate MIN_MAX for all variables                                                                                                 */
    /*                                                                                                                                              */
    /*      12.  Populate frequency of Missing and Non-missing for all variables                                                                    */
    /*                                                                                                                                              */
    /*      13.  Populate table level statistics for all tables                                                                                     */
    /*                                                                                                                                              */
    /*             OBSERVATIONS                                                                                                                     */
    /*             FILESIZE                                                                                                                         */
    /*             NUMBER_VARIABLES                                                                                                                 */
    /*             RECORD_LENGTH                                                                                                                    */
    /*                                                                                                                                              */
    /*      14.  Populate all formats                                                                                                               */
    /*                                                                                                                                              */
    /*      15.  Populate staistics for all numeric variables                                                                                       */
    /*           n nmiss min q1 median mean std q3 cv max                                                                                           */
    /*                                                                                                                                              */
    /*      16.  Populate count distinct for all variables                                                                                          */
    /*                                                                                                                                              */
    /*      17.  Populate the top 30 most frequent values for all variables                                                                         */
    /*                                                                                                                                              */
    /*      18.  Populate  cdm_010mta130sas wirh all the programs                                                                                   */
    /*                                                                                                                                              */
    /*              Variables in Creation Order                                                                                                     */
    /*                                                                                                                                              */
    /*           #    Variable         Type    Len                                                                                                  */
    /*                                                                                                                                              */
    /*           1    SDTM_PROGRAM     Char     24                                                                                                  */
    /*           2    SAS_STATEMENT    Char    113                                                                                                  */
    /*           3    LINE_NUMBER      Num       3                                                                                                  */
    /*                                                                                                                                              */
    /************************************************************************************************************************************************/
    /*                                                                                                                                              */
    /* CHANGE HISTORY                                                                                                                               */
    /*                                                                                                                                              */
    /*      Date          Programmer                Comments  Phone         Address                                                                 */
    /*                                                                                                                                              */
    /*  1.  03/09/2022    rdeangelis@gmail.com      Creation  301 655 9489  5349 N Fort Yuma Trail Tucson AZ 85750                                  */
    /*                                                                                                                                              */
    /************************************************************************************************************************************************/

    %*stop_submission;

    /*       _ _
    (_)_ __ (_) |_
    | | `_ \| | __|
    | | | | | | |_
    |_|_| |_|_|\__|

    options sasautos=("d:/cdm/oto" sasautos);

    x 'tree "d:/cdm" /F /A | clip';  * tre into clipboard;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*  C02 files are a duplicate of C01 for this example                                                                     */
    /*                                                                                                                        */
    /*  D:\CDM                                                                                                                */
    /*  |                                                                                                                     */
    /*  +---adm                                                                                                               */
    /*  |       c01adm_adae.sas7bdat                                                                                          */
    /*  |       c01adm_adef.sas7bdat                                                                                          */
    /*  |       c01adm_adsl.sas7bdat                                                                                          */
    /*  |       c01adm_adte.sas7bdat                                                                                          */
    /*  |       c02adm_adae.sas7bdat                                                                                          */
    /*  |       c02adm_adef.sas7bdat                                                                                          */
    /*  |       c02adm_adsl.sas7bdat                                                                                          */
    /*  |       c02adm_adte.sas7bdat                                                                                          */
    /*  |                                                                                                                     */
    /*  +---fmt                                                                                                               */
    /*  |       c01raw_rawfmt.sas7bcat                                                                                        */
    /*  |       c02raw_rawfmt.sas7bcat                                                                                        */
    /*  |                                                                                                                     */
    /*  +---oto                                                                                                               */
    /*  |       array.sas                                                                                                     */
    /*  |       arraydelete.sas                                                                                               */
    /*  |       cln_010.sas                                                                                                   */
    /*  |       cln_100makedefine.sas                                                                                         */
    /*  |       cln_110mkesdtmdy.sas                                                                                          */
    /*  |       cln_120mkesrtodr.sas                                                                                          */
    /*  |       cln_130cfb.sas                                                                                                */
    /*  |       cln_140mkeMtyTbl.sas                                                                                          */
    /*  |       cln_150domainx.sas                                                                                            */
    /*  |       cln_160dtc2dt.sas                                                                                             */
    /*  |       cln_170mrgsup.sas                                                                                             */
    /*  |       debug.sas                                                                                                     */
    /*  |       dosubl.sas                                                                                                    */
    /*  |       do_over.sas                                                                                                   */
    /*  |       left.sas                                                                                                      */
    /*  |       odsoff.sas                                                                                                    */
    /*  |       odson.sas                                                                                                     */
    /*  |       oto_voodoo.sas                                                                                                */
    /*  |       qcmpres.sas                                                                                                   */
    /*  |       qleft.sas                                                                                                     */
    /*  |       qtrim.sas                                                                                                     */
    /*  |       stop_submission.sas                                                                                           */
    /*  |       trim.sas                                                                                                      */
    /*  |       tut_sly.sas                                                                                                   */
    /*  |       utlfkil.sas                                                                                                   */
    /*  |       utlnopts.sas                                                                                                  */
    /*  |       utlopts.sas                                                                                                   */
    /*  |       utlrulr.sas                                                                                                   */
    /*  |       utlvenn.sas                                                                                                   */
    /*  |       utl_close.sas                                                                                                 */
    /*  |       utl_dirlst.sas                                                                                                */
    /*  |       utl_dirMac.sas                                                                                                */
    /*  |       utl_fdelete.sas                                                                                               */
    /*  |       utl_logcurchk.sas                                                                                             */
    /*  |       utl_mergsupp.sas                                                                                              */
    /*  |       utl_mrgSup.sas                                                                                                */
    /*  |       utl_optlen.sas                                                                                                */
    /*  |       utl_optlenpos.sas                                                                                             */
    /*  |       utl_perpac.sas                                                                                                */
    /*  |       utl_perpacBkp.sas                                                                                             */
    /*  |       utl_readpipe.sas                                                                                              */
    /*  |       utl_remLblQuo.sas                                                                                             */
    /*  |       utl_rens.sas                                                                                                  */
    /*  |       utl_submit_pl64.sas                                                                                           */
    /*  |       utl_submit_py64_38.sas                                                                                        */
    /*  |       utl_submit_r32.sas                                                                                            */
    /*  |       utl_submit_r64.sas                                                                                            */
    /*  |       utl_varlist.sas                                                                                               */
    /*  |       utl_ymrlan100.sas                                                                                             */
    /*  |       xpy.sas                                                                                                       */
    /*  |                                                                                                                     */
    /*  +---raw                                                                                                               */
    /*  |       c01raw_rawadverse.sas7bdat                                                                                    */
    /*  |       c01raw_rawdemographic.sas7bdat                                                                                */
    /*  |       c01raw_rawdosing.sas7bdat                                                                                     */
    /*  |       c01raw_rawlabs.sas7bdat                                                                                       */
    /*  |       c01raw_rawpain.sas7bdat                                                                                       */
    /*  |       c02raw_rawadverse.sas7bdat                                                                                    */
    /*  |       c02raw_rawdemographic.sas7bdat                                                                                */
    /*  |       c02raw_rawdosing.sas7bdat                                                                                     */
    /*  |       c02raw_rawlabs.sas7bdat                                                                                       */
    /*  |       c02raw_rawpain.sas7bdat                                                                                       */
    /*  |                                                                                                                     */
    /*  +---sas                                                                                                               */
    /*  |       c01adm_AE.sas                                                                                                 */
    /*  |       c01adm_EF.sas                                                                                                 */
    /*  |       c01adm_SL.sas                                                                                                 */
    /*  |       c01adm_TE.sas                                                                                                 */
    /*  |       c01sdm_Ae.sas                                                                                                 */
    /*  |       c01sdm_Dm.sas                                                                                                 */
    /*  |       c01sdm_Ex.sas                                                                                                 */
    /*  |       c01sdm_Lb.sas                                                                                                 */
    /*  |       c01sdm_Xp.sas                                                                                                 */
    /*  |       c02adm_AE.sas                                                                                                 */
    /*  |       c02adm_EF.sas                                                                                                 */
    /*  |       c02adm_SL.sas                                                                                                 */
    /*  |       c02adm_TE.sas                                                                                                 */
    /*  |       c02sdm_Ae.sas                                                                                                 */
    /*  |       c02sdm_Dm.sas                                                                                                 */
    /*  |       c02sdm_Ex.sas                                                                                                 */
    /*  |       c02sdm_Lb.sas                                                                                                 */
    /*  |       c02sdm_Xp.sas                                                                                                 */
    /*  |       clx_010mta.sas                                                                                                */
    /*  |                                                                                                                     */
    /*  +---sd1                         * sdm, adm and raw copied here for convience                                          */
    /*  |       c01adm_adae.sas7bdat                                                                                          */
    /*  |       c01adm_adef.sas7bdat                                                                                          */
    /*  |       c01adm_adsl.sas7bdat                                                                                          */
    /*  |       c01adm_adte.sas7bdat                                                                                          */
    /*  |       c01raw_rawadverse.sas7bdat                                                                                    */
    /*  |       c01raw_rawdemographic.sas7bdat                                                                                */
    /*  |       c01raw_rawdosing.sas7bdat                                                                                     */
    /*  |       c01raw_rawlabs.sas7bdat                                                                                       */
    /*  |       c01raw_rawpain.sas7bdat                                                                                       */
    /*  |       c01sdm_dm.sas7bdat                                                                                            */
    /*  |       c01sdm_ex.sas7bdat                                                                                            */
    /*  |       c01sdm_lb.sas7bdat                                                                                            */
    /*  |       c01sdm_xp.sas7bdat                                                                                            */
    /*  |       c02adm_adae.sas7bdat                                                                                          */
    /*  |       c02adm_adef.sas7bdat                                                                                          */
    /*  |       c02adm_adsl.sas7bdat                                                                                          */
    /*  |       c02adm_adte.sas7bdat                                                                                          */
    /*  |       c02raw_rawadverse.sas7bdat                                                                                    */
    /*  |       c02raw_rawdemographic.sas7bdat                                                                                */
    /*  |       c02raw_rawdosing.sas7bdat                                                                                     */
    /*  |       c02raw_rawlabs.sas7bdat                                                                                       */
    /*  |       c02raw_rawpain.sas7bdat                                                                                       */
    /*  |       c01sdm_ae.sas7bdat                                                                                            */
    /*  |       c02sdm_ae.sas7bdat                                                                                            */
    /*  |       c02sdm_dm.sas7bdat                                                                                            */
    /*  |       c02sdm_ex.sas7bdat                                                                                            */
    /*  |       c02sdm_lb.sas7bdat                                                                                            */
    /*  |       c02sdm_xp.sas7bdat                                                                                            */
    /*  |                                                                                                                     */
    /*  +---sdm                                                                                                               */
    /*  |       c01sdm_ae.sas7bdat                                                                                            */
    /*  |       c01sdm_dm.sas7bdat                                                                                            */
    /*  |       c01sdm_ex.sas7bdat                                                                                            */
    /*  |       c01sdm_lb.sas7bdat                                                                                            */
    /*  |       c01sdm_xp.sas7bdat                                                                                            */
    /*  |       c02sdm_ae.sas7bdat                                                                                            */
    /*  |       c02sdm_dm.sas7bdat                                                                                            */
    /*  |       c02sdm_ex.sas7bdat                                                                                            */
    /*  |       c02sdm_lb.sas7bdat                                                                                            */
    /*  |       c02sdm_xp.sas7bdat                                                                                            */
    /*  |                                                                                                                     */
    /*  +---xls                                                                                                               */
    /*  |       cln_adamMetadata.xlsx                                                                                         */
    /*  |       cln_sdtmMetadata.xlsx                                                                                         */
    /*  |                                                                                                                     */
    /*  +---xpt                                                                                                               */
    /*  |   |   c01sdm_Ae.xpt                                                                                                 */
    /*  |   |   c01sdm_Dm.xpt                                                                                                 */
    /*  |   |   c01sdm_Ex.xpt                                                                                                 */
    /*  |   |   c01sdm_Lb.xpt                                                                                                 */
    /*  |   |   c01sdm_Xp.xpt                                                                                                 */
    /*  |   |   c02sdm_Ae.xpt                                                                                                 */
    /*  |   |   c02sdm_Dm.xpt                                                                                                 */
    /*  |   |   c02sdm_Ex.xpt                                                                                                 */
    /*  |   |   c02sdm_Lb.xpt                                                                                                 */
    /*  |   |   c02sdm_Xp.xpt                                                                                                 */
    /*  |   |   co1sdm_suppdm.xpt                                                                                             */
    /*  |   \   co2sdm_suppdm.xpt                                                                                             */
    /*  |                                                                                                                     */
    /*  \---zip                                                                                                               */
    /*          clx.zip                                                                                                       */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*                       _                  _ _
      ___ ___  _ __  _   _  | |_ ___    ___  __| / |
     / __/ _ \| `_ \| | | | | __/ _ \  / __|/ _` | |
    | (_| (_) | |_) | |_| | | || (_) | \__ \ (_| | |
     \___\___/| .__/ \__, |  \__\___/  |___/\__,_|_|
              |_|    |___/
    */

    * done to siplifiy coding ;

    libname cdm    "d:\cdm" ;

    libname cdmAdm "d:\cdm\adm" access=readonly ;
    libname cdmRaw "d:\cdm\raw" access=readonly ;
    libname cdmSdm "d:\cdm\sdm" access=readonly ;

    libname cdmSd1 "d:\cdm\sd1" ;

    proc copy in=cdmAdm  out=cdmSd1; run;quit;
    proc copy in=cdmRaw  out=cdmSd1; run;quit;
    proc copy in=cdmSdm  out=cdmSd1; run;quit;

    /*   _             _
     ___| |_ __ _ _ __| |_
    / __| __/ _` | `__| __|
    \__ \ || (_| | |  | |_
    |___/\__\__,_|_|   \__|

    */

    * incase you accidentally run the entire code - you need to rum this a section at a time;

    options sasautos=("C:\Cloud\Box\SAS_DA\SAS_Development\JSI_1187_01\DA\DP\SAS\macros_rjd" sasautos);

    libname cdm    "d:/cdm";
    libname cdmsd1 "d:/cdm/sdm";

    * clear the work directory;
    proc datasets lib=work kill nodetails nolist mt=data;
    run;quit;

    /*

    * delete interim datasets if you rerun;
    proc datasets lib=cdm nodetails nolist;
      delete
          cdm_010mta010
          cdm_010mta020
          cdm_010mta030
          cdm_010mta040
          cdm_010mta050
          cdm_010mta060
          cdm_010mta070
          cdm_010mta080
          cdm_010mta090
          cdm_010mta100
          cdm_010mta110
          cdm_010mta130
       ;
    run;quit;

    * delete macro aarrays;
    %utlnopts;
    %arraydelete(_tbl   )
    %arraydelete(_var   )
    %arraydelete(_tblchr)
    %arraydelete(_varchr)
    %arraydelete(_tblnum)
    %arraydelete(_varnum)
    %utlopts;
    */

    /*
     _                       _       _
    | |_ ___ _ __ ___  _ __ | | __ _| |_ ___
    | __/ _ \ `_ ` _ \| `_ \| |/ _` | __/ _ \
    | ||  __/ | | | | | |_) | | (_| | ||  __/
     \__\___|_| |_| |_| .__/|_|\__,_|\__\___|
                      |_|
    */

    * Structure of Commom Meta data Model;
    * asking question of the meta data;

    %let atr=%str(
    length
       Src           $16      /* SAS CRF DDT SDM DMP - Source for meta data */
       Study         $32
       Table         $32
       Variable      $32
       Question      $32     /* question see below for some of the questions */
       Answer        $8192;
    label
       Src      = "SAS CRF DDT SDM DMP - Source for meta data"
       Study    = "Study Identifier"
       Table    = "Clinical Dataset CRT/SDTM/ADAM"
       Variable = "Table SAS Dataset/CRF Module.."
       Question = "Question ie Label, Number of Obs and Max Min"
       Answer   = "Answer ie number of obs, location of library..";
     keep
       Src
       Study
       Table
       Variable
       Question
       Answer;);


    /*           _   _
      ___  _ __ | |_(_) ___  _ __  ___
     / _ \| `_ \| __| |/ _ \| `_ \/ __|
    | (_) | |_) | |_| | (_) | | | \__ \
     \___/| .__/ \__|_|\___/|_| |_|___/
          |_|
    */

    options compress=binary validvarname=upcase ls=255;

    /*_  __    _    ____ ____   ___       _    ____  ____      _ __   ______
    |  \/  |  / \  / ___|  _ \ / _ \     / \  |  _ \|  _ \    / \\ \ / / ___|
    | |\/| | / _ \| |   | |_) | | | |   / _ \ | |_) | |_) |  / _ \\ V /\___ \
    | |  | |/ ___ \ |___|  _ <| |_| |  / ___ \|  _ <|  _ <  / ___ \| |  ___) |
    |_|  |_/_/   \_\____|_| \_\\___/  /_/   \_\_| \_\_| \_\/_/   \_\_| |____/
                                          _        _     _
     _ __ __ ___      __  ___  __ _ ___  | |_ __ _| |__ | | ___  ___
    | `__/ _` \ \ /\ / / / __|/ _` / __| | __/ _` | `_ \| |/ _ \/ __|
    | | | (_| |\ V  V /  \__ \ (_| \__ \ | || (_| | |_) | |  __/\__ \
    |_|  \__,_| \_/\_/   |___/\__,_|___/  \__\__,_|_.__/|_|\___||___/

    */

    * these arrays wiil be used over and over again;

    %utl_dirlst(d:/cdm/sd1,cdm_010mtaSd1);

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*  Up to 40 obs WORK.cdm_010MTASD1 total obs=28 07MAR2022:15:18:2                                                        */
    /*                                                                                                                        */
    /*     FILENAME                            FILENAME                FILENAME                                               */
    /*                                                                                                                        */
    /*     RAW date to Map                     SDTMs                   ADAM                                                   */
    /*                                                                                                                        */
    /*     c01raw_rawadverse.sas7bdat          c01sdm_ae.sas7bdat      c01adm_adae.sas7bdat                                   */
    /*     c01raw_rawdemographic.sas7bdat      c01sdm_dm.sas7bdat      c01adm_adef.sas7bdat                                   */
    /*     c01raw_rawdosing.sas7bdat           c01sdm_ex.sas7bdat      c01adm_adsl.sas7bdat                                   */
    /*     c01raw_rawlabs.sas7bdat             c01sdm_lb.sas7bdat      c01adm_adte.sas7bdat                                   */
    /*     c01raw_rawpain.sas7bdat             c01sdm_xp.sas7bdat      c02adm_adae.sas7bdat                                   */
    /*     c02raw_rawadverse.sas7bdat          c02sdm_ae.sas7bdat      c02adm_adef.sas7bdat                                   */
    /*     c02raw_rawdemographic.sas7bdat      c02sdm_dm.sas7bdat      c02adm_adsl.sas7bdat                                   */
    /*     c02raw_rawdosing.sas7bdat           c02sdm_ex.sas7bdat      c02adm_adte.sas7bdat                                   */
    /*     c02raw_rawlabs.sas7bdat             c02sdm_lb.sas7bdat                                                             */
    /*     c02raw_rawpain.sas7bdat             c02sdm_xp.sas7bdat                                                             */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    data cdm_010mtaLst;
      length sd1 $32;
      set cdm_010mtaSd1;
      if index(filename,'sas7bdat')>0;
      sd1=upcase(cats(scan(filename,1,"."))) ;
      keep sd1;
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*  Up to 40 obs WORK.cdm_010MTALST total obs=28 07MAR2022:15:21:46                                                       */
    /*                                                                                                                        */
    /*  SD1                            SD1       AD1                                                                          */
    /*                                                                                                                        */
    /*  RAW date to Map               SDTMs      ADAMs                                                                        */
    /*                                                                                                                        */
    /*  c01raw_rawadverse          c01sdm_ae     c01adm_adae                                                                  */
    /*  c01raw_rawdemographic      c01sdm_dm     c01adm_adef                                                                  */
    /*  c01raw_rawdosing           c01sdm_ex     c01adm_adsl                                                                  */
    /*  c01raw_rawlabs             c01sdm_lb     c01adm_adte                                                                  */
    /*  c01raw_rawpain             c01sdm_xp     c02adm_adae                                                                  */
    /*  c02raw_rawadverse          c02sdm_ae     c02adm_adef                                                                  */
    /*  c02raw_rawdemographic      c02sdm_dm     c02adm_adsl                                                                  */
    /*  c02raw_rawdosing           c02sdm_ex     c02adm_adte                                                                  */
    /*  c02raw_rawlabs             c02sdm_lb                                                                                  */
    /*  c02raw_rawpain             c02sdm_xp                                                                                  */
    /*                                                                                                                        */
    /***************************************************************************************************************************

    /*
     _ __ ___   __ _  ___ _ __ ___     __ _ _ __ _ __ __ _ _   _ ___
    | `_ ` _ \ / _` |/ __| `__/ _ \   / _` | `__| `__/ _` | | | / __|
    | | | | | | (_| | (__| | | (_) | | (_| | |  | | | (_| | |_| \__ \
    |_| |_| |_|\__,_|\___|_|  \___/   \__,_|_|  |_|  \__,_|\__, |___/
         _ _      _   _               |___/
      __| (_) ___| |_(_) ___  _ __   __ _ _ __ _   _
     / _` | |/ __| __| |/ _ \| `_ \ / _` | `__| | | |
    | (_| | | (__| |_| | (_) | | | | (_| | |  | |_| |
     \__,_|_|\___|\__|_|\___/|_| |_|\__,_|_|   \__, |
                                               |___/
    */

    * bootstrap from meta data;

    proc sql;
     create
           table cdm.cdm_010mta010  as
     select
           'CDM_SCHEMA'                 as Src      length 32
          ,memname                      as Table    length=32
          ,name                         as Variable length=32
          ,upcase(coalesce(label,name)) as label    length=8192
          ,scan(memname,1,'_')          as study    length=32
          ,coalesce(format,"NULL")      as format   length=8192
          ,coalesce(informat,"NULL")    as informat length=8192
          ,upcase(type)                 as type     length=8192
          ,put(varnum,best.)            as varnum   length=8192
          ,length
     from
           sashelp.vcolumn
     where
          libname="CDMSD1"
    ;quit;

    /************************************************************************************************************************************************/
    /*                                                                                                                                              */
    /*Up to 40 obs from cdm.cdm_010MTA010 total obs=462 09MAR2022:09:22:49                                                                          */
    /*                                                                                                                                              */
    /*Obs       SRC           TABLE       VARIABLE    LABEL                                  STUDY     FORMAT       INFORMAT    TYPE    LENGTH      */
    /*                                                                                                                                              */
    /*  1    cdm_SCHEMA    C01ADM_ADAE    STUDYID     STUDY IDENTIFIER                       C01ADM    NULL           NULL      CHAR       15       */
    /*  2    cdm_SCHEMA    C01ADM_ADAE    USUBJID     UNIQUE SUBJECT IDENTIFIER              C01ADM    NULL           NULL      CHAR       25       */
    /*  3    cdm_SCHEMA    C01ADM_ADAE    SITEID      STUDY SITE IDENTIFIER                  C01ADM    NULL           NULL      CHAR        7       */
    /*  4    cdm_SCHEMA    C01ADM_ADAE    COUNTRY     COUNTRY                                C01ADM    NULL           NULL      CHAR        3       */
    /*  5    cdm_SCHEMA    C01ADM_ADAE    AESEQ       SEQUENCE NUMBER                        C01ADM    NULL           NULL      NUM         8       */
    /*  6    cdm_SCHEMA    C01ADM_ADAE    AGE         UNIQUE SUBJECT IDENTIFIER              C01ADM    NULL           NULL      NUM         8       */
    /* ...                                                                                                                                          */
    /************************************************************************************************************************************************/

    /* _           _     _        _     _
      (_)_   _ ___| |_  | |_ __ _| |__ | | ___  ___
      | | | | / __| __| | __/ _` | `_ \| |/ _ \/ __|
      | | |_| \__ \ |_  | || (_| | |_) | |  __/\__ \
     _/ |\__,_|___/\__|  \__\__,_|_.__/|_|\___||___/
    |__/
    */

    proc sql noprint;
      *reset inobs=3;
      select
         distinct
          Table
      into
          :_jusTbl1-
      from
          cdm.cdm_010mta010
    ;quit;

    %let _jusTbln=&sqlobs;

    %let _endtbl=&&_jusTbl&_jusTbln;

    %utlnopts;
    %put &=_jusTbln Table for each variable;
    %put &=_jusTbl1 First table in macro array;
    %put &=_endTbl  Last table in macro array;
    %utlopts;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* _JUSTBLN=28                   Tables                                                                                   */
    /*                                                                                                                        */
    /* _JUSTBL1=C01RAW_RAWADVERSE    First table in macro array                                                               */
    /* _ENDTBL=C02SDM_XP             Last table in macro array                                                                */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*
     __   ____ _ _ __ ___
     \ \ / / _` | `__/ __|
      \ V / (_| | |  \__ \
       \_/ \__,_|_|  |___/

    */

    proc sql noprint;
      *reset inobs=3;
      select
          Table
         ,Variable
      into
          :_tbl1-
         ,:_var1-
      from
          cdm.cdm_010mta010
    ;quit;

    %let _tbln=&sqlobs;
    %let _varn=&sqlobs;

    %let _endtbl=&&_tbl&_tbln;
    %let _endvar=&&_var&_varn;

    %utlnopts;
    %put &=_tbln    Table for each variable;
    %put &=_varn    Number of Variables;
    %put &=_tbl1    First table in macro array;
    %put &=_endtbl     Last table in macro array;
    %put &=_var1    First Variable in macro array;
    %put &=_endvar     Last variable in macro array;
    %utlopts;


    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*    MACRO ARRAY RAW VARIABLES WITH 462 VARIABLE NAMES AND ASSOCIATED TABLE NAMES                                        */
    /*                                                                                                                        */
    /*                                                                                                                        */
    /*    _TBLN=462                  Tables for each variable                                                                 */
    /*    _VARN=462                  Number of Variables                                                                      */
    /*    _TBL1=C01RAW_RAWADVERSE    First table in macro array                                                               */
    /*    _ENDTBL=C02SDM_XP          Last table in macro array                                                                */
    /*    _VAR1=SUBJECT              First Variable in macro array                                                            */
    /*    _ENDVAR=XPSTRESN           Last variable in macro array                                                             */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*      _
       ___| |__   __ _ _ __  __   ____ _ _ __ ___
      / __| `_ \ / _` | `__| \ \ / / _` | `__/ __|
     | (__| | | | (_| | |     \ V / (_| | |  \__ \
      \___|_| |_|\__,_|_|      \_/ \__,_|_|  |___/

    */

    proc sql noprint;
      *reset inobs=3;
      select
          table
         ,Variable
      into
          :_tblchr1-
         ,:_varchr1-
      from
          cdm.cdm_010mta010
      where
          upcase(type)="CHAR"
    ;quit;

    %let _tblchrn=&sqlobs;
    %let _varchrn=&sqlobs;

    %let _endtbl=&&_tblchr&_tblchrn;
    %let _endvar=&&_varchr&_varchrn;

    %utlnopts;
    %put &=_tblchrn    Table for each char ble;
    %put &=_varchrn    Number of Char Variables;
    %put &=_tblchr1    First table in char macro array;
    %put &=_endtbl     Last table in char macro array;
    %put &=_varchr1    First char Variable in macro array;
    %put &=_endvar     Last chr variable in macro array;
    %utlopts;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* _TBLCHRN=292           Tables for each char ble                                                                        */
    /* _VARCHRN=292           Number of Char Variables                                                                        */
    /*                                                                                                                        */
    /* _TBLCHR1=RAWADVERSE    First table in char macro array                                                                 */
    /* _ENDTBL=C02SDM_XP      Last table in char macro array                                                                  */
    /*                                                                                                                        */
    /* _VARCHR1=BODYSYS       First char Variable in macro array                                                              */
    /* _ENDVAR=XPSTRESC       Last chr variable in macro array                                                                */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*
      _ __  _   _ _ __ ___   __   ____ _ _ __ ___
     | `_ \| | | | `_ ` _ \  \ \ / / _` | `__/ __|
     | | | | |_| | | | | | |  \ V / (_| | |  \__ \
     |_| |_|\__,_|_| |_| |_|   \_/ \__,_|_|  |___/

    */

    proc sql noprint;
      *reset inobs=3;
      select
          table
         ,Variable
      into
          :_tblnum1-
         ,:_varnum1-
      from
          cdm.cdm_010mta010
      where
          upcase(type)="NUM"
    ;quit;

    %let _tblnumn=&sqlobs;
    %let _varnumn=&sqlobs;

    %let _endtbl=&&_tblnum&_tblnumn;
    %let _endvar=&&_varnum&_varnumn;

    %utlnopts;
    %put &=_tblnumn    Table for each num ble;
    %put &=_varnumn    Number of num variables;
    %put &=_tblnum1    First table in num macro array;
    %put &=_endtbl     Last table in num macro array;
    %put &=_varnum1    First num variable in macro array;
    %put &=_endvar     Last num variable in macro array;
    %utlopts;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* _TBLNUMN=170            Table for each num ble                                                                         */
    /* _VARNUMN=170            Number of num variables                                                                        */
    /*                                                                                                                        */
    /* _TBLNUM1=RAWADVERSE    First table in num macro array                                                                  */
    /* _ENDTBL=C02SDM_XP      Last table in num macro array                                                                   */
    /*                                                                                                                        */
    /* _VARNUM1=SUBJECT       First num variable in macro array                                                               */
    /* _ENDVAR=XPSTRESN       Last num variable in macro array                                                                */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* Available macro arrays                                                                                                 */
    /*                                                                                                                        */
    /*       _jusTbl#                                                                                                         */
    /*                                                                                                                        */
    /*       _tbl#      all tables and vars                                                                                   */
    /*       _var#                                                                                                            */
    /*                                                                                                                        */
    /*       _tblchr#   tables with char vars                                                                                 */
    /*       _varchr#   char vars in tables                                                                                   */
    /*                                                                                                                        */
    /*       _tblnum#   tables with char vars                                                                                 */
    /*       _varnum#   char vars in tables                                                                                   */
    /*                                                                                                                        */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*              _
      ___ _ __   __| |   __ _ _ __ _ __ __ _ _   _ ___
     / _ \ `_ \ / _` |  / _` | `__| `__/ _` | | | / __|
    |  __/ | | | (_| | | (_| | |  | | | (_| | |_| \__ \
     \___|_| |_|\__,_|  \__,_|_|  |_|  \__,_|\__, |___/
         _             _                    _ |___/
     ___| |_ __ _ _ __| |_   _ __ ___   ___| |_ __ _
    / __| __/ _` | `__| __| | `_ ` _ \ / _ \ __/ _` |
    \__ \ || (_| | |  | |_  | | | | | |  __/ || (_| |
    |___/\__\__,_|_|   \__| |_| |_| |_|\___|\__\__,_|

    */

    * add new meta data;

    data cdm.cdm_010mta020  ;

     &atr;

       set cdm.cdm_010mta010 end=fin;

       Src='CDM_SCHEMA';

       Question=cats(substr(study,4),"_LABEL"   );  Answer=label                    ;output;
       Question=cats(substr(study,4),"_FORMAT"  );  Answer=format                   ;output;
       Question=cats(substr(study,4),"_INFORMAT");  Answer=informat                 ;output;
       Question=cats(substr(study,4),"_TYPE"    );  Answer=upcase(type)             ;output;
       Question=cats(substr(study,4),"_LENGTH"  );  Answer=strip(put(length,??7.))  ;output;
       Question=cats(substr(study,4),"_VARNUM"  );  Answer=left(varnum)             ;output;

    run;quit;

    /*************************************************************************************************************************/
    /*                                                                                                                       */
    /* Up to 40 obs from CDM.CDM_010MTA020 total obs=2,772 14SEP2022:09:20:35                                                */
    /*                                                                                                                       */
    /*  Obs       SRC        STUDY        TABLE       VARIABLE    QUESTION        ANSWER                                     */
    /*                                                                                                                       */
    /*    1    cdm_SCHEMA    C01ADM    C01ADM_ADAE    STUDYID     ADM_LABEL       STUDY IDENTIFIER                           */
    /*    2    cdm_SCHEMA    C01ADM    C01ADM_ADAE    STUDYID     ADM_FORMAT      NULL                                       */
    /*    3    cdm_SCHEMA    C01ADM    C01ADM_ADAE    STUDYID     ADM_INFORMAT    NULL                                       */
    /*    4    cdm_SCHEMA    C01ADM    C01ADM_ADAE    STUDYID     ADM_TYPE        CHAR                                       */
    /*    5    cdm_SCHEMA    C01ADM    C01ADM_ADAE    STUDYID     ADM_LENGTH      15                                         */
    /*    6    cdm_SCHEMA    C01ADM    C01ADM_ADAE    STUDYID     ADM_VARNUM      1                                          */
    /*    7    cdm_SCHEMA    C01ADM    C01ADM_ADAE    USUBJID     ADM_LABEL       UNIQUE SUBJECT IDENTIFIER                  */
    /*    8    cdm_SCHEMA    C01ADM    C01ADM_ADAE    USUBJID     ADM_FORMAT      NULL                                       */
    /*    9    cdm_SCHEMA    C01ADM    C01ADM_ADAE    USUBJID     ADM_INFORMAT    NULL                                       */
    /*                                                                                                                       */
    /*************************************************************************************************************************/
    /*
     _                _
    | | _____   _____| |___
    | |/ _ \ \ / / _ \ / __|
    | |  __/\ V /  __/ \__ \
    |_|\___| \_/ \___|_|___/

    */

    * may have to run twice;
    proc datasets lib=work nodetails nolist;
      delete cdm_010MtaTpl;
    run;quit;

    data cdm_010MtaTpl;
      length Study $32 Table  $32 Variable  $32 NLevels NMissLevels NNonMissLevels 8 ;
      call missing(of _all_);
      stop;
    run;

     /**************************************************************************************************************************/
     /*                                                                                                                        */
     /*  Contents of cdm_010MtaTpl                                                                                             */
     /*                                                                                                                        */
     /*  The CONTENTS Procedure                                                                                                */
     /*                                                                                                                        */
     /*  Data Set Name        WORK.CDM_010MTATPL            Observations          0                                            */
     /*  Member Type          DATA                          Variables             6                                            */
     /*  Engine               V9                            Indexes               0                                            */
     /*  Created              09/14/2022 08:49:49           Observation Length    120                                          */
     /*  Last Modified        09/14/2022 08:49:49           Deleted Observations  0                                            */
     /*  Protection                                         Compressed            NO                                           */
     /*  Data Set Type                                      Sorted                NO                                           */
     /*                                                                                                                        */
     /*      Variables in Creation Order                                                                                       */
     /*                                                                                                                        */
     /*  #    Variable          Type    Len                                                                                    */
     /*                                                                                                                        */
     /*  1    STUDY             Char     32                                                                                    */
     /*  2    TABLE             Char     32                                                                                    */
     /*  3    VARIABLE          Char     32                                                                                    */
     /*  4    NLEVELS           Num       8                                                                                    */
     /*  5    NMISSLEVELS       Num       8                                                                                    */
     /*  6    NNONMISSLEVELS    Num       8                                                                                    */
     /*                                                                                                                        */
     /**************************************************************************************************************************/

    %odsoff;
            proc datasets lib=work nodetails nolist;
      delete cdm_010MtaTpl;
    run;quit;

    %do_over(_jusTbl,phrase=%str(

         *ods select nlevels;

         ods output nlevels=?all;

         proc freq nlevels data=cdmSD1.?;
         tables _all_ /missing;
         run;quit;
         proc datasets lib=work nodetails nolist;
            delete cdm_010MtaTplAdd;
         run;quit;

         data cdm_010MtaTplAdd;
           length study Table Variable $32;
           retain Table "?";
           study=substr("?",1,6);
           set ?all(rename=TABLEVAR=Variable);
           Table=upcase("?");
         run;quit;

         proc datasets lib=cdmSd1 nodetails nolist;
           delete ?all;
         run;quit;

         proc append base= cdm_010MtaTpl data=cdm_010MtaTplAdd(drop=tablevarlabel);
         run;quit;
       ));

    %odson;


    /************************************************************************************************************************************************/
    /*                                                                                                                                              */
    /*  Up to 40 obs from CDM_010MTATPL total obs=462 14SEP2022:09:37:52                                                                            */
    /*                                                                                                                                              */
    /*  Obs    STUDY        TABLE       VARIABLE    NLEVELS    NMISSLEVELS    NNONMISSLEVELS                                                        */
    /*                                                                                                                                              */
    /*    1    C01ADM    C01ADM_ADAE    STUDYID         1           0                1                                                              */
    /*    2    C01ADM    C01ADM_ADAE    USUBJID        20           0               20                                                              */
    /*    3    C01ADM    C01ADM_ADAE    SITEID          5           0                5                                                              */
    /*    4    C01ADM    C01ADM_ADAE    COUNTRY         1           0                1                                                              */
    /*    5    C01ADM    C01ADM_ADAE    AESEQ           3           0                3                                                              */
    /*    6    C01ADM    C01ADM_ADAE    AGE            15           0               15                                                              */
    /*    7    C01ADM    C01ADM_ADAE    AGEGR1N         2           0                2                                                              */
    /*    8    C01ADM    C01ADM_ADAE    AGEGR1          2           0                2                                                              */
    /*    9    C01ADM    C01ADM_ADAE    SEX             2           0                2                                                              */
    /*   10    C01ADM    C01ADM_ADAE    TRTAN           2           1                1                                                              */
    /*                                                                                                                                              */
    /************************************************************************************************************************************************/

    /* add new levels to meta data */
    data cdm.cdm_010mta030 ;
      &atr;
     do until(fin);
      set cdm.cdm_010mta020  end=fin;
      src = upcase(src);
      output;
     end;
     do until (dne);
      set cdm_010MtaTpl end=dne;
        Src='CDM_SCHEMA';
        Table=cats(Study,'_',substr(Table,8));
        Question=cats(substr(study,4),'_LEVELS_N_MISS_NONMISS');
        Answer=cats(put(nlevels,12.),'@',put(nmisslevels,12.),'@',put(nnonmisslevels,12.));
        output;
     end;
     stop;
    run;

    /************************************************************************************************************************************************/
    /*                                                                                                                                              */
    /*   last 41 obs from CDM.CDM_010MTA020 total obs=2,772 14SEP2022:09:41:57                                                                      */
    /*                                                                                                                                              */
    /*    Obs       SRC        STUDY       TABLE      VARIABLE    QUESTION        ANSWER                                                            */
    /*                                                                                                                                              */
    /*   2732    cdm_SCHEMA    C02SDM    C02SDM_XP    XPORRES     SDM_FORMAT      NULL                                                              */
    /*   2733    cdm_SCHEMA    C02SDM    C02SDM_XP    XPORRES     SDM_INFORMAT    NULL                                                              */
    /*   2734    cdm_SCHEMA    C02SDM    C02SDM_XP    XPORRES     SDM_TYPE        CHAR                                                              */
    /*   2735    cdm_SCHEMA    C02SDM    C02SDM_XP    XPORRES     SDM_LENGTH      200                                                               */
    /*   2736    cdm_SCHEMA    C02SDM    C02SDM_XP    XPORRES     SDM_VARNUM      7                                                                 */
    /*   2737    cdm_SCHEMA    C02SDM    C02SDM_XP    VISITNUM    SDM_LABEL       VISIT NUMBER                                                      */
    /*   2738    cdm_SCHEMA    C02SDM    C02SDM_XP    VISITNUM    SDM_FORMAT      NULL                                                              */
    /*   2739    cdm_SCHEMA    C02SDM    C02SDM_XP    VISITNUM    SDM_INFORMAT    NULL                                                              */
    /*                                                                                                                                              */
    /************************************************************************************************************************************************/

    /*         _
     _ __ ___ (_)_ __    _ __ ___   __ ___  __
    | `_ ` _ \| | `_ \  | `_ ` _ \ / _` \ \/ /
    | | | | | | | | | | | | | | | | (_| |>  <
    |_| |_| |_|_|_| |_| |_| |_| |_|\__,_/_/\_\

    */

    * initialize for append;

    proc datasets lib=work nodetails nolist;
      delete cdm_010mta040 maxmin;
    run;quit;

    %utlnopts;

    %put &_tbln;
    %put &_varn;

    %do_over(_tbl _var, phrase=%str(

        * so you do not add the previous one;

        proc datasets lib=work nolist nodetails;
          delete  cdm_010mta040One;
        run;quit;

        proc sql;
          create
              table cdm_010mta040One as
          select
             'CDM_SCHEMA'                           as src
            ,scan("?_tbl",1,'_')                    as study
            ,"?_tbl"                                as table    length=32
            ,"?_var"                                as variable length=32
            ,cats(substr("?_tbl",4,3),"_MIN_MAX")   as question length=32
            ,catx("@",min(?_var), max(?_var) )      as answer   length=8192
          from
             cdmSD1.?_tbl
       ;quit;

        proc append base=cdm_010mta040 data=cdm_010mta040One;
        run;quit;

    ));

    %utlopts;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*Up to 40 obs from cdm_010MTA040 total obs=462 09MAR2022:13:29:54                                                        */
    /*                                                                                                                        */
    /*Obs       SRC        STUDY        TABLE       VARIABLE     QUESTION      ANSWER                                         */
    /*                                                                                                                        */
    /*  1    CLS_SCHEMA    C01ADM    C01ADM_ADAE    STUDYID     ADM_MIN_MAX    XYZ123@XYZ123                                  */
    /*  2    CLS_SCHEMA    C01ADM    C01ADM_ADAE    USUBJID     ADM_MIN_MAX    UNI101@UNI507                                  */
    /*  3    CLS_SCHEMA    C01ADM    C01ADM_ADAE    SITEID      ADM_MIN_MAX    100@500                                        */
    /*  4    CLS_SCHEMA    C01ADM    C01ADM_ADAE    COUNTRY     ADM_MIN_MAX    USA@USA                                        */
    /*  5    CLS_SCHEMA    C01ADM    C01ADM_ADAE    AESEQ       ADM_MIN_MAX    1@3                                            */
    /*  6    CLS_SCHEMA    C01ADM    C01ADM_ADAE    AGE         ADM_MIN_MAX    29@70                                          */
    /*  7    CLS_SCHEMA    C01ADM    C01ADM_ADAE    AGEGR1N     ADM_MIN_MAX    1@2                                            */
    /*  8    CLS_SCHEMA    C01ADM    C01ADM_ADAE    AGEGR1      ADM_MIN_MAX    <55 YEARS@>=55 YEARS                           */
    /*  9    CLS_SCHEMA    C01ADM    C01ADM_ADAE    SEX         ADM_MIN_MAX    B@W                                            */
    /* 10    CLS_SCHEMA    C01ADM    C01ADM_ADAE    TRTAN       ADM_MIN_MAX    0@0                                            */
    /* 11    CLS_SCHEMA    C01ADM    C01ADM_ADAE    TRTA        ADM_MIN_MAX    Active@Placebo                                 */
    /* 12    CLS_SCHEMA    C01ADM    C01ADM_ADAE    AETERM      ADM_MIN_MAX    ABDOMINAL CRAMPS@URINARY BL                    */
    /*                                                                                                                        */
    /**************************************************************************************************************************/
    * add maxmin;

    data cdm.cdm_010mta050 ;
      &atr;
     do until(fin);
      set cdm.cdm_010mta030  end=fin;
      src = upcase(src);
      output;
     end;
     do until (dne);
      set cdm_010mta040 end=dne;
        Src='cdm_SCHEMA';
        output;
     end;
     stop;
    run;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*  last 41 obs from CDM.CDM_010MTA050 total obs=3,696 14SEP2022:09:45:20                                                 */
    /*                                                                                                                        */
    /*   Obs       SRC        STUDY       TABLE      VARIABLE     QUESTION      ANSWER                                        */
    /*                                                                                                                        */
    /*  3656    cdm_SCHEMA    C02SDM    C02SDM_EX    EXDOSFRM    SDM_MIN_MAX    TABLET, COATED@TABLET, COATED                 */
    /*  3657    cdm_SCHEMA    C02SDM    C02SDM_EX    EXDOSTOT    SDM_MIN_MAX    1@3                                           */
    /*  3658    cdm_SCHEMA    C02SDM    C02SDM_EX    EXSTDTC     SDM_MIN_MAX    2010-01-01@2010-10-02                         */
    /*  3659    cdm_SCHEMA    C02SDM    C02SDM_EX    EXENDTC     SDM_MIN_MAX    2010-01-05@2010-12-27                         */
    /*  3660    cdm_SCHEMA    C02SDM    C02SDM_EX    EXSTDY      SDM_MIN_MAX    1@253                                         */
    /*  3661    cdm_SCHEMA    C02SDM    C02SDM_EX    EXENDY      SDM_MIN_MAX    4@335                                         */
    /*  3662    cdm_SCHEMA    C02SDM    C02SDM_LB    STUDYID     SDM_MIN_MAX    XYZ123@XYZ123                                 */
    /*  3663    cdm_SCHEMA    C02SDM    C02SDM_LB    DOMAIN      SDM_MIN_MAX    LB@LB                                         */
    /*  3664    cdm_SCHEMA    C02SDM    C02SDM_LB    USUBJID     SDM_MIN_MAX    UNI101@UNI204                                 */
    /*  3665    cdm_SCHEMA    C02SDM    C02SDM_LB    LBSEQ       SDM_MIN_MAX    1@30                                          */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*                                                  _
      _ __   ___  _ __    _ __ ___ (_)___
     | `_ \ / _ \| `_ \  | `_ ` _ \| / __|
     | |_) | (_) | |_) | | | | | | | \__ \
     | .__/ \___/| .__/  |_| |_| |_|_|___/
     |_|         |_|
    */

    proc format;

      value num2mis
       . = 'MIS'
       other='POP'
       ;

       value $chr2mis
       'Unknown',' ','UNK','U','NA','UNKNOWN','Missing','MISSING','MISS' ='MIS'
       other='POP'
        ;
    run;

    %utlnopts;

    %odsoff;

    %do_over(_justbl, phrase=%str(
        ods output onewayfreqs=?_a;
        proc freq data=cdmSD1.?;
          format _character_ $chr2mis. _numeric_ num2mis.;
          tables _all_/missing;
        run;quit;
    ));

    %odson;

    /*******************************************************************************************************************************************************************/
    /*                                                                                                                                                                 */
    /* Up to 40 obs WORK.C02SDM_XP_A total obs=13 08MAR2022:06:55:28                                                                                                   */
    /*                                                                                                                                                                 */
    /* Obs      TABLE       F_STUDYID  STUDYID  FREQUENCY  PERCENT  CUMFREQUENCY  CUMPERCENT  F_DOMAIN  DOMAIN  F_USUBJID  USUBJID  F_XPSEQ  XPSEQ  F_XPTESTCD         */
    /*                                                                                                                                                                 */
    /*   1  Table STUDYID      POP     XYZ123      177     100.000       177        100.000                                                    .                       */
    /*   2  Table DOMAIN                           177     100.000       177        100.000     POP       XP                                   .                       */
    /*   3  Table USUBJID                          177     100.000       177        100.000                        POP     UNI101              .                       */
    /*   4  Table XPSEQ                            177     100.000       177        100.000                                           POS      1                       */
    /*   5  Table XPTESTCD                         177     100.000       177        100.000                                                    .       POP             */
    /*   6  Table XPTEST                           177     100.000       177        100.000                                                    .                       */
    /*   7  Table XPORRES                          177     100.000       177        100.000                                                    .                       */
    /*   8  Table VISITNUM                          60      33.898        60         33.898                                                    .                       */
    /*                                                                                                                                                                 */
    /*******************************************************************************************************************************************************************/

    * initialize for append;
    proc datasets lib=work nodetails nolist;
      delete xpopmis;
    run;quit;

    %utlnopts;

    %do_over(_justbl, phrase=%str(
    data mpofix ;
       &atr;
       set ?_a;
       src = upcase(src);
       study=scan("?",1,'_');
       src='CDM_SCHEMA';
       question=cats(substr("?",4,3),'_MIS_POP');
       Tablex="?" ;
       Variablex =  cats('F_',scan( table , 2 , ' '));
       value    = vvaluex(Variablex) ;
       select (value);
         when ('MIS') answer=cats('MIS@',put(frequency,8.));
         when ('POP') answer=cats('POP@',put(frequency,8.));
       end;
       variable=substr(variablex,3);
       table=tablex;
    run;

    proc datasets lib=work nodetails nolist;
      delete ?_a;
    run;quit;

    proc append data=mpofix base=xpopmis ;
    run;quit;
    ));

    %utlopts;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*  last 41 obs from XPOPMIS total obs=510 14SEP2022:09:53:12                                                             */
    /*                                                                                                                        */
    /*  Obs       SRC        STUDY       TABLE      VARIABLE     QUESTION      ANSWER                                         */
    /*                                                                                                                        */
    /*  470    CDM_SCHEMA    C02SDM    C02SDM_EX    EXDOSTOT    SDM_MIS_POP    POP@84                                         */
    /*  471    CDM_SCHEMA    C02SDM    C02SDM_EX    EXSTDTC     SDM_MIS_POP    POP@84                                         */
    /*  472    CDM_SCHEMA    C02SDM    C02SDM_EX    EXENDTC     SDM_MIS_POP    POP@84                                         */
    /*  473    CDM_SCHEMA    C02SDM    C02SDM_EX    EXSTDY      SDM_MIS_POP    POP@84                                         */
    /*  474    CDM_SCHEMA    C02SDM    C02SDM_EX    EXENDY      SDM_MIS_POP    POP@84                                         */
    /*  475    CDM_SCHEMA    C02SDM    C02SDM_LB    STUDYID     SDM_MIS_POP    POP@300                                        */
    /*  476    CDM_SCHEMA    C02SDM    C02SDM_LB    DOMAIN      SDM_MIS_POP    POP@300                                        */
    /*  477    CDM_SCHEMA    C02SDM    C02SDM_LB    USUBJID     SDM_MIS_POP    POP@300                                        */
    /*                                                                                                                        */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    proc sql;
     create
       table mispop as
     select
       coalesce(l.Src      ,r.Src      )  as Src
      ,coalesce(l.Study    ,r.Study    )  as Study
      ,coalesce(l.Table      ,r.Table  )  as Table
      ,coalesce(l.Variable ,r.Variable )  as Variable
      ,coalesce(l.Question ,r.Question )  as Question
      ,cats(Coalesce(l.Answer,'MIS@0'),'#',Coalesce(r.Answer,'POP@0') )  as Answer  length=8192
     from
      xpopmis(where=(answer =: 'MIS' )) as l full outer join xpopmis(where=(answer =: 'POP' ))  as r
     on
      l.Table=r.Table
      and l.Variable = r.Variable
    ;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* Concat mising with populated                                                                                           */
    /*                                                                                                                        */
    /* Up to 40 obs WORK.MISPOP total obs=462 08MAR2022:07:57:18                                                              */
    /*                                                                                                                        */
    /* Obs       SRC        STUDY     TABLE                    VARIABLE      QUESTION      ANSWER                             */
    /*                                                                                                                        */
    /*   1    cdm_SCHEMA    C01RAW    C01RAW_RAWADVERSE        AEACN        RAW_MIS_POP    MIS@0#POP@40                       */
    /*   2    cdm_SCHEMA    C01RAW    C01RAW_RAWADVERSE        AEACTION     RAW_MIS_POP    MIS@0#POP@40                       */
    /*   3    cdm_SCHEMA    C01RAW    C01RAW_RAWADVERSE        AEBODSYS     RAW_MIS_POP    MIS@0#POP@40                       */
    /*   4    cdm_SCHEMA    C01RAW    C01RAW_RAWADVERSE        AEDECOD      RAW_MIS_POP    MIS@3#POP@40                       */
    /*   5    cdm_SCHEMA    C01RAW    C01RAW_RAWADVERSE        AEEND        RAW_MIS_POP    MIS@8#POP@32                       */
    /*   6    cdm_SCHEMA    C01RAW    C01RAW_RAWADVERSE        AEENDTC      RAW_MIS_POP    MIS@0#POP@40                       */
    /*   7    cdm_SCHEMA    C01RAW    C01RAW_RAWADVERSE        AEREL        RAW_MIS_POP    MIS@0#POP@40                       */
    /*   8    cdm_SCHEMA    C01RAW    C01RAW_RAWADVERSE        AESER        RAW_MIS_POP    MIS@0#POP@40                       */
    /*   9    cdm_SCHEMA    C01RAW    C01RAW_RAWADVERSE        AESEV        RAW_MIS_POP    MIS@3#POP@40                       */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    %utlopts;
    %odson;

    data cdm.cdm_010mta060;
      &atr;
     do until(fin);
      set cdm.cdm_010mta050     end=fin;
      src = upcase(src);
      Table=upcase(Table);
      output;
     end;
     do until (dne);
      set mispop end=dne;
        Src='CDM_SCHEMA';
        output;
     end;
     stop;
    run;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* last 41 obs from CDM.CDM_010MTA060 total obs=4,158 14SEP2022:09:56:19                                                  */
    /*                                                                                                                        */
    /*  Obs       SRC        STUDY       TABLE      VARIABLE     QUESTION          ANSWER                                     */
    /*                                                                                                                        */
    /* 4118    CDM_SCHEMA    C02SDM    C02SDM_EX    EXSEQ       SDM_MIS_POP    MIS@0#POP@84                                   */
    /* 4119    CDM_SCHEMA    C02SDM    C02SDM_EX    EXSTDTC     SDM_MIS_POP    MIS@0#POP@84                                   */
    /* 4120    CDM_SCHEMA    C02SDM    C02SDM_EX    EXSTDY      SDM_MIS_POP    MIS@0#POP@84                                   */
    /* 4121    CDM_SCHEMA    C02SDM    C02SDM_EX    EXTRT       SDM_MIS_POP    MIS@0#POP@84                                   */
    /* 4122    CDM_SCHEMA    C02SDM    C02SDM_EX    STUDYID     SDM_MIS_POP    MIS@0#POP@84                                   */
    /* 4123    CDM_SCHEMA    C02SDM    C02SDM_EX    USUBJID     SDM_MIS_POP    MIS@0#POP@84                                   */
    /* 4124    CDM_SCHEMA    C02SDM    C02SDM_LB    DOMAIN      SDM_MIS_POP    MIS@0#POP@300                                  */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*
      _        _     _
     | |_ __ _| |__ | | ___  ___
     | __/ _` | `_ \| |/ _ \/ __|
     | || (_| | |_) | |  __/\__ \
      \__\__,_|_.__/|_|\___||___/

    */

    data xmstable;

       &atr;
       set sashelp.vtable(where=(libname="CDMSD1" and memtype="DATA"));

       src    = "cdm_SCHEMA";
       Table =  MEMNAME;
       Variable= cats("***TABLE_LEVEL***");
       study  = scan(MEMNAME,1,'_');

       pfx=cats(substr(memname,4,3));

       question=cats(pfx,"_OBSERVATIONS"    ); answer=cats(nobs)     ; output ;
       question=cats(pfx,"_FILESIZE"        ); answer=cats(filesize) ; output ;
       question=cats(pfx,"_NUMBER_VARIABLES"); answer=cats(nvar)     ; output ;
       question=cats(pfx,"_RECORD_LENGTH"   ); answer=cats(nlobs)    ; output ;
       question=cats(pfx,"_MODATE"          ); answer=cats(put(modate,e8601dt.)); output ;

    run;quit;

    data cdm.cdm_010mta070 ;
      &atr;
     do until(fin);
      set cdm.cdm_010mta060    end=fin;
      output;
     end;
     do until (dne);
      set xmstable end=dne;
        if answer ne "" then output;
     end;
     stop;
    run;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* last 41 obs from CDM.CDM_010MTA070 total obs=4,298 14SEP2022:09:57:46                                                  */
    /*                                                                                                                        */
    /*  Obs    SRC        STUDY     TABLE                        VARIABLE         QUESTION                ANSWER              */
    /*                                                                                                                        */
    /* 4258 cdm_SCHEMA    C02RAW    C02RAW_RAWDEMOGRAPHIC    ***TABLE_LEVEL***    RAW_MODATE              2022-09-14T08:34:14 */
    /* 4259 cdm_SCHEMA    C02RAW    C02RAW_RAWDOSING         ***TABLE_LEVEL***    RAW_OBSERVATIONS        84                  */
    /* 4260 cdm_SCHEMA    C02RAW    C02RAW_RAWDOSING         ***TABLE_LEVEL***    RAW_FILESIZE            131072              */
    /* 4261 cdm_SCHEMA    C02RAW    C02RAW_RAWDOSING         ***TABLE_LEVEL***    RAW_NUMBER_VARIABLES    5                   */
    /* 4262 cdm_SCHEMA    C02RAW    C02RAW_RAWDOSING         ***TABLE_LEVEL***    RAW_RECORD_LENGTH       84                  */
    /* 4263 cdm_SCHEMA    C02RAW    C02RAW_RAWDOSING         ***TABLE_LEVEL***    RAW_MODATE              2022-09-14T08:34:14 */
    /* 4264 cdm_SCHEMA    C02RAW    C02RAW_RAWLABS           ***TABLE_LEVEL***    RAW_OBSERVATIONS        300                 */
    /* 4265 cdm_SCHEMA    C02RAW    C02RAW_RAWLABS           ***TABLE_LEVEL***    RAW_FILESIZE            131072              */
    /* 4266 cdm_SCHEMA    C02RAW    C02RAW_RAWLABS           ***TABLE_LEVEL***    RAW_NUMBER_VARIABLES    10                  */
    /*                                                                                                                        */
    /**************************************************************************************************************************/
    /*
       __                            _
      / _| ___  _ __ _ __ ___   __ _| |_ ___
     | |_ / _ \| `__| `_ ` _ \ / _` | __/ __|
     |  _| (_) | |  | | | | | | (_| | |_\__ \
     |_|  \___/|_|  |_| |_| |_|\__,_|\__|___/

    */


    %*inc "F:\cdm\fmt\f04\formats.sas";

    %utl_dirlst(d:/cdm/fmt,fmt);

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* Up to 40 obs WORK.FMT total obs=2 08MAR2022:08:35:26                                                                   */
    /*                                                                                                                        */
    /*                                  NUMBER_OF_    MEMBER_                                                                 */
    /* Obs           FILENAME             MEMBERS      NUMBER                                                                 */
    /*                                                                                                                        */
    /*  1     co1raw_rawfmt.sas7bcat         2           1                                                                    */
    /*  2     co2raw_rawfmt.sas7bcat         2           2                                                                    */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    proc sql;
       select
          upcase(scan(filename,1,'.'))
       into
          :_fmt1-
       from
          fmt
    ;quit;

    %put &=_fmt2;

    proc datasets lib=work nodetails nolist;
      delete allFmt;
    run;quit;

    %utlnopts;

    libname fmt "d:/cdm/fmt";

    %do_over(_fmt,phrase=%str(
       proc format cntlout=? lib=fmt.?;
       run;quit;
       data ?;
         &atr;
         retain answer;
         set ?;
         by fmtname notsorted;
         src="CDM_SCHEMA";
         study=scan("?",1,'_');
         if type="C" then Variable=cats("$",fmtname,'F');
         else Variable=cats("@",substr(fmtname,2),'F');;
         question="RAW_USER_FORMAT";
         Table=cats(study,"_",FMTNAME,'_FMT');
         if label="" then label="NA";
         if start="" then start="NA";
         if first.fmtname then do;
            answer=catx("@",cats(start,'#',end,'#',label));
         end;
         else answer=catx("@",answer,cats(start,'#',end,'#',label));
         if last.fmtname then do;
            output;
            answer="";
          end;
       run;quit;
       proc append data=?FMT base=allFmt;
       run;quit;
       ));

    %utlopts;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* Up to 40 obs WORK.ALLFMT total obs=11 14SEP2022:10:20:13                                                               */
    /*                                                                                                                        */
    /* Obs       SRC        STUDY     TABLE                         VARIABLE       QUESTION      ANSWER                       */
    /*                                                                                                                        */
    /*   1    cdm_SCHEMA    C01RAW    C01RAW_AGEGR1N_FMT            @GEGR1NF    RAW_USER_FORMAT  0#54#1@55#HIGH#2             */
    /*   2    cdm_SCHEMA    C01RAW    C01RAW_AGEGR1__FMT            @GEGR1_F    RAW_USER_FORMAT  1#1#<55 YEARS@2#2#>=55 YEARS */
    /*   3    cdm_SCHEMA    C01RAW    C01RAW_AVISITN_FMT            @VISITNF    RAW_USER_FORMAT  1#1#3@2#2#6                  */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    data cdm.cdm_010mta080 ;
      &atr;
     do until(fin);
      set cdm.cdm_010mta070 end=fin;
      src="CDM_SCHEMA";
      output;
     end;
     do until (dne);
      set allFmt end=dne;
        src="CDM_SCHEMA";
        output;
     end;
     stop;
    run;quit;


    /**************************************************************************************************************************/
    /* last 41 obs from CDM.CDM_010MTA080 total obs=4,309 14SEP2022:10:27:22                                                  */
    /*                                                                                                                        */
    /*  Obs       SRC        STUDY     TABLE               VARIABLE             QUESTION             ANSWER                   */
    /*                                                                                                                        */
    /* 4269    CDM_SCHEMA    C02RAW    C02RAW_RAWPAIN      ***TABLE_LEVEL***    RAW_OBSERVATIONS     60                       */
    /* 4270    CDM_SCHEMA    C02RAW    C02RAW_RAWPAIN      ***TABLE_LEVEL***    RAW_FILESIZE         131072                   */
    /* 4271    CDM_SCHEMA    C02RAW    C02RAW_RAWPAIN      ***TABLE_LEVEL***    RAW_NUMBER_VARIABLES 8                        */
    /* 4272    CDM_SCHEMA    C02RAW    C02RAW_RAWPAIN      ***TABLE_LEVEL***    RAW_RECORD_LENGTH    60                       */
    /* 4273    CDM_SCHEMA    C02RAW    C02RAW_RAWPAIN      ***TABLE_LEVEL***    RAW_MODATE           2022-09-14T08:34:14      */
    /* 4274    CDM_SCHEMA    C02SDM    C02SDM_AE           ***TABLE_LEVEL***    SDM_OBSERVATIONS     40                       */
    /* 4275    CDM_SCHEMA    C02SDM    C02SDM_AE           ***TABLE_LEVEL***    SDM_FILESIZE         131072                   */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*                                _      _
     _ __ ___   ___  __ _ _ __    ___| |_ __| |
    | `_ ` _ \ / _ \/ _` | `_ \  / __| __/ _` |
    | | | | | |  __/ (_| | | | | \__ \ || (_| |
    |_| |_| |_|\___|\__,_|_| |_| |___/\__\__,_|

    */

    proc datasets lib=work nodetails nolist;
      delete xmastatx;
    run;quit;

    %utlnopts;

    %odsoff;

    %do_over(_tblnum _varnum,phrase=%str(

        proc datasets lib=work nodetails nolist;
          delete xma_stat xma_statxpo xma_varadd;
        run;quit;

        proc means data=cdmsd1.?_tblnum;
        var ?_varnum;
        output out=xma_stat n=n nmiss=nmiss min=min q1=q1 median= median mean= mean std= std q3= q3 cv= cv max= max;
        run;quit;

        proc transpose data=xma_stat out=xma_statxpo(where=(index(_name_,'_')=0 ));
        run;quit;

        data xma_stats;
          &atr;
          retain Table "?_tblnum" src "cdm_SCHEMA" Variable "?_varnum";
          set xma_statxpo;
          question=cats(substr("?_tblnum",4,3),'_',_name_);
          study=scan("?_tblnum",1,'_');
          answer=cats(col1);
        run;quit;

        proc append data=xma_stats base=xmastatx ;
        run;quit;
    ));

    %odson;
    %utlopts;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* last 41 obs from XMASTATX total obs=1,700 14SEP2022:10:29:50                                                           */
    /*                                                                                                                        */
    /*  Obs       SRC        STUDY       TABLE      VARIABLE    QUESTION      ANSWER                                          */
    /*                                                                                                                        */
    /* 1660    cdm_SCHEMA    C02SDM    C02SDM_LB    LBDY        SDM_MAX       193                                             */
    /* 1661    cdm_SCHEMA    C02SDM    C02SDM_XP    XPSEQ       SDM_N         177                                             */
    /* 1662    cdm_SCHEMA    C02SDM    C02SDM_XP    XPSEQ       SDM_NMISS     0                                               */
    /* 1663    cdm_SCHEMA    C02SDM    C02SDM_XP    XPSEQ       SDM_MIN       1                                               */
    /* 1664    cdm_SCHEMA    C02SDM    C02SDM_XP    XPSEQ       SDM_Q1        1                                               */
    /* 1665    cdm_SCHEMA    C02SDM    C02SDM_XP    XPSEQ       SDM_MEDIAN    2                                               */
    /* 1666    cdm_SCHEMA    C02SDM    C02SDM_XP    XPSEQ       SDM_MEAN      1.988700565                                     */
    /* 1667    cdm_SCHEMA    C02SDM    C02SDM_XP    XPSEQ       SDM_STD       0.8187344764                                    */
    /* 1668    cdm_SCHEMA    C02SDM    C02SDM_XP    XPSEQ       SDM_Q3        3                                               */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    data cdm.cdm_010mta090;
      &atr;
     do until(fin);
      set cdm.cdm_010mta070    end=fin;
      src="CDM_SCHEMA";
      output;
     end;
     do until (dne);
      set xmastatx end=dne;
      src="CDM_SCHEMA";
      output;
     end;
     stop;
    run;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* last 41 obs from CDM.CDM_010MTA090 total obs=5,998 14SEP2022:10:32:54                                                  */
    /*                                                                                                                        */
    /*  Obs       SRC        STUDY       TABLE      VARIABLE    QUESTION      ANSWER                                          */
    /*                                                                                                                        */
    /* 5958    CDM_SCHEMA    C02SDM    C02SDM_LB    LBDY        SDM_MAX       193                                             */
    /* 5959    CDM_SCHEMA    C02SDM    C02SDM_XP    XPSEQ       SDM_N         177                                             */
    /* 5960    CDM_SCHEMA    C02SDM    C02SDM_XP    XPSEQ       SDM_NMISS     0                                               */
    /* 5961    CDM_SCHEMA    C02SDM    C02SDM_XP    XPSEQ       SDM_MIN       1                                               */
    /* 5962    CDM_SCHEMA    C02SDM    C02SDM_XP    XPSEQ       SDM_Q1        1                                               */
    /* 5963    CDM_SCHEMA    C02SDM    C02SDM_XP    XPSEQ       SDM_MEDIAN    2                                               */
    /* 5964    CDM_SCHEMA    C02SDM    C02SDM_XP    XPSEQ       SDM_MEAN      1.988700565                                     */
    /* 5965    CDM_SCHEMA    C02SDM    C02SDM_XP    XPSEQ       SDM_STD       0.8187344764                                    */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*
         _ _     _   _            _
      __| (_)___| |_(_)_ __   ___| |_
     / _` | / __| __| | `_ \ / __| __|
    | (_| | \__ \ |_| | | | | (__| |_
     \__,_|_|___/\__|_|_| |_|\___|\__|

    */

    %utlnopts;

    proc datasets lib=work nodetails nolist;
      delete xmaunq;
    run;quit;

    %put &=_tbln;

    %do_over(_tbl _var,phrase=%str(

       proc datasets lib=work nodetails nolist;
         delete  xmaunqapn;
       run;quit;

       proc sql;
          create
             table xmaunqapn as
          select
            "cdm_SCHEMA"                              as Src        length=32
            ,scan("?_tbl",1,'_')                      as Study      length=32
            ,"?_tbl"                                  as Table      length=32
            ,"?_var"                                  as Variable   length=32
            ,cats(substr("?_tbl",4,3),"_DISTINCT")    as Question   length=32
            ,cats(count(distinct ?_var))              as answer     length=32
          from
             cdmsd1.?_tbl
          group by
             Src
            ,Study
            ,Table
            ,Variable
            ,Question
    ;quit;
    proc append data=xmaunqapn base=xmaunq;
    run;quit;
    ));

    %utlopts;

    data cdm.cdm_010mta100 ;
      &atr;
     do until(fin);
      set cdm.cdm_010mta090    end=fin;
      src="CDM_SCHEMA";
      output;
     end;
     do until (dne);
      set xmaunq end=dne;
        src="CDM_SCHEMA";
        output;
     end;
     stop;
    run;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* last 41 obs from CDM.CDM_010MTA100 total obs=6,460 14SEP2022:10:36:41                                                  */
    /*                                                                                                                        */
    /*  Obs       SRC        STUDY       TABLE      VARIABLE      QUESTION      ANSWER                                        */
    /*                                                                                                                        */
    /* 6420    CDM_SCHEMA    C02SDM    C02SDM_EX    EXDOSFRM    SDM_DISTINCT     1                                            */
    /* 6421    CDM_SCHEMA    C02SDM    C02SDM_EX    EXDOSTOT    SDM_DISTINCT     3                                            */
    /* 6422    CDM_SCHEMA    C02SDM    C02SDM_EX    EXSTDTC     SDM_DISTINCT     64                                           */
    /* 6423    CDM_SCHEMA    C02SDM    C02SDM_EX    EXENDTC     SDM_DISTINCT     60                                           */
    /* 6424    CDM_SCHEMA    C02SDM    C02SDM_EX    EXSTDY      SDM_DISTINCT     16                                           */
    /* 6425    CDM_SCHEMA    C02SDM    C02SDM_EX    EXENDY      SDM_DISTINCT     38                                           */
    /* 6426    CDM_SCHEMA    C02SDM    C02SDM_LB    STUDYID     SDM_DISTINCT     1                                            */
    /* 6427    CDM_SCHEMA    C02SDM    C02SDM_LB    DOMAIN      SDM_DISTINCT     1                                            */
    /*                                                                                                                        */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*__                  _                _____  ___
     / _|_ __ ___  __ _  | |_ ___  _ __   |___ / / _ \
    | |_| `__/ _ \/ _` | | __/ _ \| `_ \    |_ \| | | |
    |  _| | |  __/ (_| | | || (_) | |_) |  ___) | |_| |
    |_| |_|  \___|\__, |  \__\___/| .__/  |____/ \___/
                     |_|          |_|
    */

    %utlnopts;

    proc datasets lib=work nodetails nolist;
      delete  xmafrqapn ;
    run;quit;

    %do_over(_tbl _var,phrase=%str(

       proc datasets lib=work nodetails nolist;
         delete xmafrqout xmafix xmarol;
       run;quit;

       ods exclude all;
       ods output onewayfreqs=xmafrqout;
       proc freq data=cdmsd1.?_tbl order=freq;
         format ?_var;
         tables ?_var / missing;
       run;quit;
       ods select all;

       data xmafix;
         &atr;
         length ans $200;
         keep ans;
         retain Table "?_tbl" Variable "?_var" ;
         set xmafrqout(obs=30);
         question = cats(substr("?_tbl",4,3),"_FREQUENCIES");
         study=scan("?_tbl",1,'_');
         Variable =  cats('F_',scan( table , 2 , ' '));
         if frequency=. then frequanyy=-1;
         ans=cats(vvaluex(Variable),'=',put(frequency,8. -l));
         Variable=substr(Variable,3);

         select;
           when (ans =: '#') ans=cats('MIS',ans);
           when (ans =: '.') ans=cats('MIS',substr(ans,2));
           otherwise;
         end;
       run;quit;

       data xmarol(rename=tst=answer);
         retain src study table variable question;
         retain tst;
         length tst $8192;
         set xmafix;
         by Variable notsorted;
         tst=catx("@",tst,ans);
         table="?_tbl";
         src = "CDM_SCHEMA" ;
         if last.Variable then output;
         drop answer ans;
       run;quit;

       proc append data=xmarol base=xmafrqapn;
       run;quit;
    ));

    %odson;

    %utlopts;

    data cdm.cdm_010mta110 ;
      &atr;
     do until(fin);
      set cdm.cdm_010mta100   end=fin;
      output;
     end;
     do until (dne);
      set xmafrqapn end=dne;
        output;
     end;
     stop;
    run;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* last 41 obs from CDM.CDM_010MTA110 total obs=6,922 14SEP2022:10:41:51                                                  */
    /*                                                                                                                        */
    /*   Obs    SRC        STUDY       TABLE      VARIABLE       QUESTION       ANSWER                                        */
    /*                                                                                                                        */
    /* 15039 cdm_SCHEMA    C02SDM    C02SDM_XP    DOMAIN      SDM_FREQUENCIES   XP=177                                        */
    /* 15041 cdm_SCHEMA    C02SDM    C02SDM_XP    XPSEQ       SDM_FREQUENCIES   1=60@2=59@3=58                                */
    /* 15042 cdm_SCHEMA    C02SDM    C02SDM_XP    XPTESTCD    SDM_FREQUENCIES   XPPAIN=177                                    */
    /* 15043 cdm_SCHEMA    C02SDM    C02SDM_XP    XPTEST      SDM_FREQUENCIES   Pain Score=177                                */
    /* 15044 cdm_SCHEMA    C02SDM    C02SDM_XP    XPORRES     SDM_FREQUENCIES   Severe=58@None=53@Mild=35@Moderate=31         */
    /* 15045 cdm_SCHEMA    C02SDM    C02SDM_XP    VISITNUM    SDM_FREQUENCIES   0=60@1=59@2=58                                */
    /* 15046 cdm_SCHEMA    C02SDM    C02SDM_XP    VISIT       SDM_FREQUENCIES   baseline=60@3 months=59@6 months=58           */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*
     ___  __ _ ___   _ __   __ _ _ __ ___  ___
    / __|/ _` / __| | `_ \ / _` | `_ ` _ \/ __|
    \__ \ (_| \__ \ | |_) | (_| | | | | | \__ \
    |___/\__,_|___/ | .__/ \__, |_| |_| |_|___/
                    |_|    |___/
    */

    %utl_dirlst(d:\cdm\sas\,sas);

    %utlopts;

    data cdm_010inisas ;
      set sas;
      if lowcase(scan(filename,2,'.'))="sas";
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*  Up to 40 obs from SAS total obs=18 08MAR2022:11:13:13                                                                 */
    /*                                                                                                                        */
    /*                          NUMBER_OF_    MEMBER_                                                                         */
    /*  Obs      FILENAME         MEMBERS      NUMBER                                                                         */
    /*                                                                                                                        */
    /*    1    c01sdm_Ae.sas        18            1                                                                           */
    /*    2    c01sdm_Dm.sas        18            2                                                                           */
    /*    3    c01sdm_Ex.sas        18            3                                                                           */
    /*    4    c01sdm_Lb.sas        18            4                                                                           */
    /*    5    c01sdm_Xp.sas        18            5                                                                           */
    /*    6    c02sdm_Ae.sas        18            6                                                                           */
    /*    7    c02sdm_Dm.sas        18            7                                                                           */
    /*    8    c02sdm_Ex.sas        18            8                                                                           */
    /*    9    c02sdm_Lb.sas        18            9                                                                           */
    /*   10    c02sdm_Xp.sas        18           10                                                                           */
    /*   11    c02adm_EF.sas        18           11                                                                           */
    /*   12    c02adm_SL.sas        18           12                                                                           */
    /*   13    c02adm_TE.sas        18           13                                                                           */
    /*   14    c02sdm_Ae.sas        18           14                                                                           */
    /*   15    c02sdm_Dm.sas        18           15                                                                           */
    /*   16    c02sdm_Ex.sas        18           16                                                                           */
    /*   17    c02sdm_Lb.sas        18           17                                                                           */
    /*   18    c02sdm_Xp.sas        18           18                                                                           */
    /*                                                                                                                        */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    proc sql;
      create
         table sasfyl as
      select
         distinct
            lowcase(filename) as fylcut
           ,lowcase(cats("d:\cdm\SAS\",filename)) as filename length=128
           ,scan(filename,2,'_.')     as domain
           ,'CDMPGM'   as study

      from
         cdm_010inisas
      where
         index(filename,"FORMAT")=0
    ;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* Up to 40 obs WORK.SASFYL total obs=18 14SEP2022:10:57:02                                                               */
    /*                                                                                                                        */
    /* Obs       FYLCUT                FILENAME            DOMAIN    STUDY                                                    */
    /*                                                                                                                        */
    /*   1    c01adm_ae.sas    d:\cdm\sas\c01adm_ae.sas      AE      CDMPGM                                                   */
    /*   2    c01adm_ef.sas    d:\cdm\sas\c01adm_ef.sas      EF      CDMPGM                                                   */
    /*   3    c01adm_sl.sas    d:\cdm\sas\c01adm_sl.sas      SL      CDMPGM                                                   */
    /*   4    c01adm_te.sas    d:\cdm\sas\c01adm_te.sas      TE      CDMPGM                                                   */
    /*   5    c01sdm_ae.sas    d:\cdm\sas\c01sdm_ae.sas      Ae      CDMPGM                                                   */
    /*   6    c01sdm_dm.sas    d:\cdm\sas\c01sdm_dm.sas      Dm      CDMPGM                                                   */
    /*   7    c01sdm_ex.sas    d:\cdm\sas\c01sdm_ex.sas      Ex      CDMPGM                                                   */
    /*   8    c01sdm_lb.sas    d:\cdm\sas\c01sdm_lb.sas      Lb      CDMPGM                                                   */
    /*   9    c01sdm_xp.sas    d:\cdm\sas\c01sdm_xp.sas      Xp      CDMPGM                                                   */
    /*  10    c02adm_ae.sas    d:\cdm\sas\c02adm_ae.sas      AE      CDMPGM                                                   */
    /*  11    c02adm_ef.sas    d:\cdm\sas\c02adm_ef.sas      EF      CDMPGM                                                   */
    /*  12    c02adm_sl.sas    d:\cdm\sas\c02adm_sl.sas      SL      CDMPGM                                                   */
    /*  13    c02adm_te.sas    d:\cdm\sas\c02adm_te.sas      TE      CDMPGM                                                   */
    /*  14    c02sdm_ae.sas    d:\cdm\sas\c02sdm_ae.sas      Ae      CDMPGM                                                   */
    /*  15    c02sdm_dm.sas    d:\cdm\sas\c02sdm_dm.sas      Dm      CDMPGM                                                   */
    /*  16    c02sdm_ex.sas    d:\cdm\sas\c02sdm_ex.sas      Ex      CDMPGM                                                   */
    /*  17    c02sdm_lb.sas    d:\cdm\sas\c02sdm_lb.sas      Lb      CDMPGM                                                   */
    /*  18    c02sdm_xp.sas    d:\cdm\sas\c02sdm_xp.sas      Xp      CDMPGM                                                   */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    proc datasets lib=work nodetails nolist;
      delete cdm_pgm;
    run;quit;

    data _null_;

       set sasfyl ;

       call symputx("fyl",filename);
       call symputx("fylcut",fylcut);

       rc=dosubl('

           %put &=fylcut;
           %put &=fyl;

           proc datasets lib=work nodetails nolist;
             delete tmp;
           run;quit;

           data tmp;
             length fyl fylnam $96 lyn $200;
             infile "&fyl";
             input;
             lyn=_infile_;
             rec=_n_;
             fylnam=lowcase("&fylcut");
             fyl="&fyl";
          run;quit;

          proc append data=tmp base=cdm_pgm;
          run;quit;

          ');

    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* Up to 40 obs from CDM_PGM total obs=2,504 14SEP2022:10:58:55                                                           */
    /*                                                                                                                        */
    /*           FYL                                                        LYN                                       REC     */
    /*                                                                                                                        */
    /* d:\cdm\sas\c01adm_ae.sas  %macro cln_625adamAdae ;                                                               1     */
    /* d:\cdm\sas\c01adm_ae.sas  *---------------------------------------------------------------*;                     2     */
    /* d:\cdm\sas\c01adm_ae.sas  * ADAE.sas creates the ADaM ADAE-structured data set                                   3     */
    /* d:\cdm\sas\c01adm_ae.sas  * for AE data (ADAE), saved to the ADaM libref.                                        4     */
    /* d:\cdm\sas\c01adm_ae.sas  *---------------------------------------------------------------*;                     5     */
    /* d:\cdm\sas\c01adm_ae.sas                                                                                         6     */
    /* d:\cdm\sas\c01adm_ae.sas  **** CREATE EMPTY ADAE DATASET CALLED EMPTY_ADAE;                                      7     */
    /* d:\cdm\sas\c01adm_ae.sas                                                                                         8     */
    /* d:\cdm\sas\c01adm_ae.sas  %cln_140mkeMtyTbl(metadatafile=&gbl_root/xls/cln_adamMetadata.xlsx,dataset=ADAE);      9     */
    /* d:\cdm\sas\c01adm_ae.sas                                                                                        10     */
    /* d:\cdm\sas\c01adm_ae.sas  %put &=adaekeepstring;                                                                11     */
    /* d:\cdm\sas\c01adm_ae.sas                                                                                        12     */
    /* d:\cdm\sas\c01adm_ae.sas  proc sort                                                                             13     */
    /* d:\cdm\sas\c01adm_ae.sas    data = cln.adsl                                                                     14     */
    /* d:\cdm\sas\c01adm_ae.sas    (keep = usubjid siteid country age agegr1 agegr1n sex race trtsdt trt01a trt01an)   15     */
    /* d:\cdm\sas\c01adm_ae.sas    out = adsl;                                                                         16     */
    /* d:\cdm\sas\c01adm_ae.sas      by usubjid;                                                                       17     */
    /* d:\cdm\sas\c01adm_ae.sas  run;quit;                                                                             18     */
    /* d:\cdm\sas\c01adm_ae.sas                                                                                        19     */
    /* d:\cdm\sas\c01adm_ae.sas  data adae;                                                                            20     */
    /* d:\cdm\sas\c01adm_ae.sas    /*merge sdtm.ae (in = inae) adsl (in = inadsl); */                                  21     */
    /* d:\cdm\sas\c01adm_ae.sas    merge cln.ae (in = inae) cln.adsl (in = inadsl);                                    22     */
    /* d:\cdm\sas\c01adm_ae.sas      by usubjid ;                                                                      23     */
    /* d:\cdm\sas\c01adm_ae.sas                                                                                        24     */
    /* d:\cdm\sas\c01adm_ae.sas          if inae and not inadsl then                                                   25     */
    /* d:\cdm\sas\c01adm_ae.sas            put 'PROB' 'LEM: Subject missing from ADSL?-- ' usubjid= inae= inadsl= ;    26     */
    /* d:\cdm\sas\c01adm_ae.sas                                                                                        27     */
    /* d:\cdm\sas\c01adm_ae.sas          rename trt01a    = trta                                                       28     */
    /* d:\cdm\sas\c01adm_ae.sas                 trt01an   = trtan                                                      29     */
    /* d:\cdm\sas\c01adm_ae.sas          ;                                                                             30     */
    /* d:\cdm\sas\c01adm_ae.sas          if inadsl and inae;                                                           31     */
    /* d:\cdm\sas\c01adm_ae.sas                                                                                        32     */
    /* d:\cdm\sas\c01adm_ae.sas          %cln_160dtc2dt(aestdtc, prefix=ast, refdt=trtsdt);                            33     */
    /* d:\cdm\sas\c01adm_ae.sas          %cln_160dtc2dt(aeendtc, prefix=aen, refdt=trtsdt);                            34     */
    /* d:\cdm\sas\c01adm_ae.sas                                                                                        35     */
    /* d:\cdm\sas\c01adm_ae.sas          if index(AEDECOD, 'PAIN')>0 or AEDECOD='HEADACHE' then                        36     */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    data cdm.cdm_010mta120;

      &atr ;

      set cdm_pgm;
      by fyl notsorted;

      if first.fyl then do;
         src      = "CDM_SCHEMA";
         answer   = fyl;
         schema   = upcase(scan(fylnam,1,'_'));
         table    = upcase(scan(fylnam,1,'.'));
         study    = scan(table,1,'_');
         question = cats(upcase(substr(table,4,3)),"_PROGRAM_PATH");
         variable = "***TABLE_LEVEL***";
         output;
      end;

      src      = "CDM_SCHEMA";
      answer   = put(rec,z5.)!!' '!!trim(put(lyn,$char384.) );
      schema   = upcase(scan(fylnam,1,'_'));
      table    = upcase(scan(fylnam,1,'.'));
      study    = scan(table,1,'_');
      question = cats(upcase(substr(table,4,3)),"_PROGRAM");
      variable = "***TABLE_LEVEL***";
      output;

    run;quit;


    /*************************************************************************************************************************************************/
    /*                                                                                                                                               */
    /*                                                                                                                                               */
    /* Up to 40 obs CDM.CDM_010MTA120 total obs=2,504 14SEP2022:13:58:53                                                                             */
    /*                                                                                                                                               */
    /*     SRC       STUDY      TABLE         VARIABLE         QUESTION     ANSWER                                                                   */
    /*                                                                                                                                               */
    /*  CDM_SCHEMA   C01ADM   C01ADM_AE   ***TABLE_LEVEL***   SAS_PROGRAM_PATH  d:\cdm\sas\c01adm_ae.sas                                             */
    /*  CDM_SCHEMA   C01ADM   C01ADM_AE   ***TABLE_LEVEL***   SAS_PROGRAM   00001 %macro cln_625adamAdae ;                                           */
    /*  CDM_SCHEMA   C01ADM   C01ADM_AE   ***TABLE_LEVEL***   SAS_PROGRAM   00002 *---------------------------------------------------------------*; */
    /*  CDM_SCHEMA   C01ADM   C01ADM_AE   ***TABLE_LEVEL***   SAS_PROGRAM   00003 * ADAE.sas creates the ADaM ADAE-structured data set               */
    /*  CDM_SCHEMA   C01ADM   C01ADM_AE   ***TABLE_LEVEL***   SAS_PROGRAM   00004 * for AE data (ADAE), saved to the ADaM libref.                    */
    /*  CDM_SCHEMA   C01ADM   C01ADM_AE   ***TABLE_LEVEL***   SAS_PROGRAM   00005 *---------------------------------------------------------------*; */
    /*  CDM_SCHEMA   C01ADM   C01ADM_AE   ***TABLE_LEVEL***   SAS_PROGRAM   00006                                                                    */
    /*  CDM_SCHEMA   C01ADM   C01ADM_AE   ***TABLE_LEVEL***   SAS_PROGRAM   00007 **** CREATE EMPTY ADAE DATASET CALLED EMPTY_ADAE;                  */
    /*  CDM_SCHEMA   C01ADM   C01ADM_AE   ***TABLE_LEVEL***   SAS_PROGRAM   00008                                                                    */
    /*  CDM_SCHEMA   C01ADM   C01ADM_AE   ***TABLE_LEVEL***   SAS_PROGRAM   00009 %cln_140mkeMtyTbl(                                                 */
    /*  CDM_SCHEMA   C01ADM   C01ADM_AE   ***TABLE_LEVEL***   SAS_PROGRAM   00010    metadatafile=&gbl_root/xls/cln_adamMetadata.xlsx,dataset=ADAE)  */
    /*  CDM_SCHEMA   C01ADM   C01ADM_AE   ***TABLE_LEVEL***   SAS_PROGRAM   00011 %put &=adaekeepstring;                                             */
    /*  CDM_SCHEMA   C01ADM   C01ADM_AE   ***TABLE_LEVEL***   SAS_PROGRAM   00012                                                                    */
    /*  CDM_SCHEMA   C01ADM   C01ADM_AE   ***TABLE_LEVEL***   SAS_PROGRAM   00013 proc sort                                                          */
    /*  CDM_SCHEMA   C01ADM   C01ADM_AE   ***TABLE_LEVEL***   SAS_PROGRAM   00014   data = cln.adsl                                                  */
    /*  CDM_SCHEMA   C01ADM   C01ADM_AE   ***TABLE_LEVEL***   SAS_PROGRAM   00015   (keep = usubjid siteid country age agegr1 agegr1n sex race       */
    /*  CDM_SCHEMA   C01ADM   C01ADM_AE   ***TABLE_LEVEL***   SAS_PROGRAM   00016    sex race trtsdt trt01a trt01 out = adsl;                        */
    /*  CDM_SCHEMA   C01ADM   C01ADM_AE   ***TABLE_LEVEL***   SAS_PROGRAM   00017     by usubjid;                                                    */
    /*  CDM_SCHEMA   C01ADM   C01ADM_AE   ***TABLE_LEVEL***   SAS_PROGRAM   00018 run;quit;                                                          */
    /*                                                                                                                                               */
    /*************************************************************************************************************************************************/


    data cdm.cdm_010mta130 ;
      &atr;
     do until(fin);
      set cdm.cdm_010mta110   end=fin;
      src=upcase(src);
      output;
     end;
     do until (dne);
      set cdm.cdm_010mta120 end=dne;
        src=upcase(src);
        output;
     end;
     stop;
    run;

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */

    * optimize lengths;
    %utl_optlenpos(cdm.cdm_010mta010,cdm.cdm_010mta010);
    %utl_optlenpos(cdm.cdm_010mta020,cdm.cdm_010mta020);
    %utl_optlenpos(cdm.cdm_010mta030,cdm.cdm_010mta030);
    %utl_optlenpos(cdm.cdm_010mta040,cdm.cdm_010mta040);
    %utl_optlenpos(cdm.cdm_010mta050,cdm.cdm_010mta050);
    %utl_optlenpos(cdm.cdm_010mta060,cdm.cdm_010mta060);
    %utl_optlenpos(cdm.cdm_010mta070,cdm.cdm_010mta070);
    %utl_optlenpos(cdm.cdm_010mta080,cdm.cdm_010mta080);
    %utl_optlenpos(cdm.cdm_010mta090,cdm.cdm_010mta090);
    %utl_optlenpos(cdm.cdm_010mta100,cdm.cdm_010mta100);
    %utl_optlenpos(cdm.cdm_010mta110,cdm.cdm_010mta110);
    %utl_optlenpos(cdm.cdm_010mta120,cdm.cdm_010mta120);
    %utl_optlenpos(cdm.cdm_010mta130,cdm.cdm_010mta130);

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */
