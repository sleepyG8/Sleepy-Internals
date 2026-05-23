This ntdll export contains a pointer to a table. This table is used by the loader at startup. It includes randomization fields and cfg related fields.

Debug>>!dump

Which addr to get?

00007FFEFC74A450    // Export address from ntdll

How many bytes to read? (Press enter to read till end of function)

296                // size as defined by the first unsigned long in the struct

<< PS_SYSTEM_DLL_INIT_BLOCK struct >>
```bash
28 01 00 00 00 00 00 00 00 00 E2 2B 
00 00 00 00 00 00 56 7C FD 7F 00 00 
60 42 0D 77 00 00 00 00 A0 B5 11 77 
00 00 00 00 70 B4 11 77 00 00 00 00 
50 B5 11 77 00 00 00 00 30 B6 11 77 
00 00 00 00 30 0B 17 77 00 00 00 00 
00 00 0A 77 00 00 00 00 A0 D2 1D 77 
00 00 00 00 A8 97 1D 77 00 00 00 00 
00 00 00 00 00 00 00 00 00 00 00 00 
00 00 00 00 00 00 00 00 00 00 00 00 
00 00 00 00 00 00 00 00 00 00 00 00 
00 00 00 00 00 00 00 00 00 00 00 00        
00 00 00 00 00 00 00 00 78 5D 2E 66 
00 00 00 00 10 00 00 00 00 01 00 00 
00 00 00 00 00 00 00 10 01 20 00 00 
00 00 00 00 00 00 61 20 F5 7D 00 00 
00 00 00 00 00 02 00 00 00 00 00 00 
00 00 00 00 00 00 00 00 00 00 00 00 
00 00 00 00 00 00 00 00 00 00 00 00 
00 00 00 00 00 00 00 00 00 00 00 00 
40 71 7C FC FE 7F 00 00 C0 71 7C FC 
FE 7F 00 00 40 70 7C FC FE 7F 00 00 
C0 70 7C FC FE 7F 00 00 00 00 00 00 
00 00 00 00 00 00 00 00 00 00 00 00 
00 00 00 00 00 00 00 00
```

Entropy Checker Extention:
-------------------------------
2.2016

[+] Memory Protections: [PAGE_READONLY]


RE tip: This struct conatins a rng entry that is unique per process. Use it to increase entropy in encryption algorithms.

RE tip #2: The 2nd entry in the struct, SystemDllWowRelocation, is 0 on non wow64 processes.

Heres the struct, I actually got this struct from ntDoc, this is the same as the kernel struct so I just used that:
https://ntdoc.m417z.com/ldrsystemdllinitblock

```bash
typedef struct _PS_SYSTEM_DLL_INIT_BLOCK_V3
{
    ULONG Size;
    ULONG64 SystemDllWowRelocation; // effectively since WIN8
    ULONG64 SystemDllNativeRelocation;
    ULONG64 Wow64SharedInformation[16]; // use WOW64_SHARED_INFORMATION as index
    ULONG RngData;
    union
    {
        ULONG Flags;
        struct
        {
            ULONG CfgOverride : 1; // effectively since REDSTONE
            ULONG Reserved : 31;
        };
    };
    PS_MITIGATION_OPTIONS_MAP_V3 MitigationOptionsMap;
    ULONG64 CfgBitMap; // effectively since WINBLUE
    ULONG64 CfgBitMapSize;
    ULONG64 Wow64CfgBitMap; // effectively since THRESHOLD
    ULONG64 Wow64CfgBitMapSize;
    PS_MITIGATION_AUDIT_OPTIONS_MAP_V3 MitigationAuditOptionsMap; // effectively since REDSTONE3
    ULONG64 ScpCfgCheckFunction; // since 24H2
    ULONG64 ScpCfgCheckESFunction;
    ULONG64 ScpCfgDispatchFunction;
    ULONG64 ScpCfgDispatchESFunction;
    ULONG64 ScpArm64EcCallCheck;
    ULONG64 ScpArm64EcCfgCheckFunction;
    ULONG64 ScpArm64EcCfgCheckESFunction;
} PS_SYSTEM_DLL_INIT_BLOCK_V3, *PPS_SYSTEM_DLL_INIT_BLOCK_V3,
    PS_SYSTEM_DLL_INIT_BLOCK, *PPS_SYSTEM_DLL_INIT_BLOCK;
```
