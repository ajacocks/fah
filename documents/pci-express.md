Review PCIexpress lane negotiation: 

cat probe-nvidia.ksh
#!/bin/bash
if [ `whoami` != root ]; then
echo "Please run as root"
exit
fi
BAR="===================================================================================================="
for C in `lspci  | grep NVIDIA | grep -i geforce | cut -f1 -d" "`; do
echo $BAR
echo "Card: "`lspci -s $C`
lspci -vv -s $C | grep  -i gt
echo $BAR
done

sudo ./probe-nvidia.ksh

====================================================================================================

Card: 01:00.0 VGA compatible controller: NVIDIA Corporation TU106 [GeForce RTX 2060 Rev. A] (rev a1)
		LnkCap:	Port #0, Speed 8GT/s, Width x16, ASPM L0s L1, Exit Latency L0s <1us, L1 <4us
		LnkSta:	Speed 8GT/s (ok), Width x8 (downgraded)
		LnkCtl2: Target Link Speed: 8GT/s, EnterCompliance- SpeedDis-
		
====================================================================================================
 
Card: 02:00.0 VGA compatible controller: NVIDIA Corporation TU106 [GeForce RTX 2060 Rev. A] (rev a1)
		LnkCap:	Port #1, Speed 8GT/s, Width x16, ASPM L0s L1, Exit Latency L0s <1us, L1 <4us
		LnkSta:	Speed 8GT/s (ok), Width x8 (downgraded)
		LnkCtl2: Target Link Speed: 8GT/s, EnterCompliance- SpeedDis-
		
====================================================================================================

    Review the Link Capability. Here (8 Gigatransfer/sec) and 16 lanes, to the status: 8 GT/s and 8 lanes are shown.

================================
