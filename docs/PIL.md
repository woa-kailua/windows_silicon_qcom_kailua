# PIL Memory Region Settings Assignment


## MTP08550 Platform ID Case Study

### Windows Firmware Information

The entire PIL region **allocated** by the UEFI firmware is:

- Start: 0x8A800000
- End: 0xA2A80000
- Size: 0x18280000

(Refer to the section named UEFI Memory Map for more information on how this is defined).

### Subsections of PIL Region:

| FW Name      | MPSS       | MPSS_DTB   | IPA        | GAP0       | GFXSUC     | GAP1       | SPSS       | SPU_TZ     | SPU_MODEM  | CAMERA     | VENUS      | EVA        | CDSP       | CDSP_DTB   | ADSP_DTB   | ADSPSLPI   |
|--------------|------------|------------|------------|------------|------------|------------|------------|------------|------------|------------|------------|------------|------------|------------|------------|------------|
| Memory Set   | PGCM       | PGCM       | PGCM       | PGCM       | PGCM       | PGCM       | PGCM       | PGCM       | PGCM       | PGCM       | PGCM       | PGCM       | PGCM       | PGCM       | PGCM       | PGCM       |
| Memory Start | 0x8A800000 | 0x9B000000 | 0x9B080000 | 0x9B090000 | 0x9B09A000 | 0x9B09F000 | 0x9B100000 | 0x9B280000 | 0x9B2E0000 | 0x9B300000 | 0x9BB00000 | 0x9C200000 | 0x9C900000 | 0x9E900000 | 0x9E980000 | 0x9EA00000 |
| Memory End   | 0x9B000000 | 0x9B080000 | 0x9B090000 | 0x9B09A000 | 0x9B09F000 | 0x9B100000 | 0x9B280000 | 0x9B2E0000 | 0x9B300000 | 0x9BB00000 | 0x9C200000 | 0x9C900000 | 0x9E900000 | 0x9E980000 | 0x9EA00000 | 0xA2A80000 |
| Memory Size  | 0x10800000 | 0x00080000 | 0x00010000 | 0x0000A000 | 0x00005000 | 0x00061000 | 0x00180000 | 0x00060000 | 0x00020000 | 0x00800000 | 0x00700000 | 0x00700000 | 0x02000000 | 0x00080000 | 0x00080000 | 0x04080000 |
| Config       | PILE       | PILE       | PILE       | PILE       | PILE       | PILE       | PILE       | PILE       | PILE       | PILE       | PILE       | PILE       | PILE       | PILE       | PILE       | PILE       |

PGCM area is configured in PILE (qcpilEXT8550) and must match above table allocation plan.

**Below regions are hardcoded in ACPI tables and are therefore not dynamically used by the Operating System**

**Below regions are not hardcoded in ACPI tables / firmware and are therefore dynamically used by the Operating System**

For this kind of region, the PIL driver is instructed the total size of the region in use dynamically below using "PGCM":

- PGCM:	  Start 0x8A800000, End 0xA2A80000, Size 0x18280000
  - Defined in \components\QC8550\Extensions\HexagonLoader\PIL\qcpilEXT8550.inf

We then define every firmware binary meant to load in such region:

We reached the end of the whole reserved region in our UEFI firmware.

---

### INF Packages

\components\QC8550\Extensions\HexagonLoader\PIL\qcpilEXT8550.inf

```ini
;
```

### UEFI Memory Map

```c
/* ... */
{"GPU PRR",           0x81F00000, 0x00010000, AddMem, MEM_RES, WRITE_COMBINEABLE, Reserv, UNCACHED_UNBUFFERED_XN},
/* ... */
{"MPSS_EFS",          0x85300000, 0x00280000, AddMem, SYS_MEM, SYS_MEM_CAP, Reserv, UNCACHED_UNBUFFERED_XN},
/* ... */
{"PIL Reserved",      0x8A800000, 0x18280000, AddMem, MEM_RES, UNCACHEABLE, Reserv, UNCACHED_UNBUFFERED_XN},
/* ... */
```

### ACPI DSDT

```asl
//
```


### Android Firmware Information

#### Device Tree

```dts
mpss_region@89800000 {
	no-map;
	reg = <0x00 0x8a800000 0x00 0x10800000>;
	phandle = <0x75>;
};

q6_mpss_dtb_region@9b000000 {
	no-map;
	reg = <0x00 0x9b000000 0x00 0x80000>;
	phandle = <0x76>;
};

ipa_fw_region@9b080000 {
	no-map;
	reg = <0x00 0x9b080000 0x00 0x10000>;
	phandle = <0x2e1>;
};

ipa_gsi_region@9b090000 {
	no-map;
	reg = <0x00 0x9b090000 0x00 0xa000>;
	phandle = <0x2e2>;
};

gpu_micro_code_region@9b09a000 {
	no-map;
	reg = <0x00 0x9b09a000 0x00 0x2000>;
	phandle = <0x2e3>;
};

spss_region_region@9b100000 {
	no-map;
	reg = <0x00 0x9b100000 0x00 0x180000>;
	phandle = <0x44>;
};

spu_tz_shared_mem@9b280000 {
	no-map;
	reg = <0x00 0x9b280000 0x00 0x60000>;
	phandle = <0x1ed>;
};

spu_modem_shared_mem@9b2e0000 {
	no-map;
	reg = <0x00 0x9b2e0000 0x00 0x20000>;
	phandle = <0x1ec>;
};

camera_region@9b300000 {
	no-map;
	reg = <0x00 0x9b300000 0x00 0x800000>;
	phandle = <0x2e4>;
};

video_region@9bb00000 {
	no-map;
	reg = <0x00 0x9bb00000 0x00 0x700000>;
	phandle = <0x2e5>;
};

cvp_region@9c200000 {
	no-map;
	reg = <0x00 0x9c200000 0x00 0x700000>;
	phandle = <0x2b1>;
};

cdsp_region@9c900000 {
	no-map;
	reg = <0x00 0x9c900000 0x00 0x2000000>;
	phandle = <0x6f>;
};

q6_cdsp_dtb_region@9e900000 {
	no-map;
	reg = <0x00 0x9e900000 0x00 0x80000>;
	phandle = <0x70>;
};

q6_adsp_dtb_region@9e980000 {
	no-map;
	reg = <0x00 0x9e980000 0x00 0x80000>;
	phandle = <0x6b>;
};

adspslpi_region@9ea00000 {
	no-map;
	reg = <0x00 0x9ea00000 0x00 0x4080000>;
	phandle = <0x6a>;
};
```

#### XBL

```ini
0x8A800000, 0x18280000, "PIL Reserved",      AddMem, MEM_RES, UNCACHEABLE, Reserv, UNCACHED_UNBUFFERED_XN
```
