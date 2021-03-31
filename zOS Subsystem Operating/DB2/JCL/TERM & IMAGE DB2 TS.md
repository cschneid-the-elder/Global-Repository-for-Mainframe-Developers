# Terminate DB2 Utility. Make a full DB2 Tablespace image copy

```
//${USERID}D JOB (${JOB_ACCOUNTING_INFO}),'COPY DB2 TS',REGION=2M,
//        MSGCLASS=H,CLASS=${JOB_CLASS}
//* TO REMOVE STATUS UT REPLACE ${UTID} WITH UTILITY ID
//TERM1     EXEC PGM=IKJEFT01,DYNAMNBR=20
//SYSTSPRT  DD SYSOUT=*
//STEPLIB   DD DSN=${DB2_SDSNLOAD},DISP=SHR
//SYSPRINT  DD SYSOUT=*
//SYSUDUMP  DD SYSOUT=*
//SYSOUT    DD SYSOUT=*
//SYSTSIN   DD *
  DSN SYSTEM(${SUBSYSTEM_NAME})
  -TERM UTILITY (${UTID})
  END
//SYSIN     DD DUMMY
//* DELETE THIS STEP IF YOU HAVE ALREADY CREATED GDG
//* GDG FOR FULL IMAGE COPY DATA SETS
//DEFGDG   EXEC PGM=IDCAMS
//SYSPRINT DD SYSOUT=*
//SYSIN    DD *
  DEFINE GDG (NAME (DSNB10.${SUBSYSTEM_NAME}.IC.@{DB2_TS_2}.@{DB2_TS_3}) +
  LIMIT(2) SCRATCH)
/*
//COPY1     EXEC DSNUPROC,SYSTEM=${SUBSYSTEM_NAME},
//             LIB='${DB2_SDSNLOAD}',
//             UID=''
//SYSCOPY   DD DSN=DSNB10.DBBG.IC.@{DB2_TS_2}.@{DB2_TS_3},
//             DISP=(NEW,CATLG),
//             SPACE=(TRK,(15,5),RLSE),
//             UNIT=SYSDA
//SYSIN     DD  *
  COPY TABLESPACE @{DB2_TS_2}.@{DB2_TS_3} DSNUM ALL
  FULL YES
```