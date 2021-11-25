# cmdbuildpro
Whasssup yall got into coding and exploits about a year ago took the next step and coded up some heat for yall. A bunch of fortnite exploits on this one anywhere to aimbot to flying. I think yall would like this one a bunch sincerly  the 12 year old codester.

(I WILL DISPLAY THE CODE FOR YOU RIGHT HERE TO SHOW U WHAT I DID BUT TO USE IT JUST DOWNLOAD THE FILE (fortnite.build.exe) or I WILL WALK YOU THROUGH ON HOW TO DO IT.

Internal
Instant Revive:

*(float*)(LocalPawn + 0x3580) = 1;
No weapon animation (cba to update):

uintptr_t CurrentWeapon = *(uintptr_t*)(LocalPawn + 0x5E8); //CurrentWeapon Offset
if(CurrentWeapon) {
    *(bool*)(CurrentWeapon + 0x2B3) = true; //bDisableEquipAnimation Offset (update this yourself)
}
RapidFire:

float a = 0;
float b = 0;
uintptr_t CurrentWeapon = *(uintptr_t*)(LocalPawn + 0x5E8); //CurrentWeapon Offset
if (CurrentWeapon) {
    a = *(float*)(CurrentWeapon + 0x9F4); //LastFireTime Offset
    b = *(float*)(CurrentWeapon + 0x9F8); //LastFireTimeVerified Offset
    *(float*)(CurrentWeapon + 0x9F4) = a + b - 0.0003; //LastFireTime Offset
}
AirStuck:

if (GetAsyncKeyState(VK_MENU)) { //Alt Keybind
    *(float*)(LocalPawn + 0x98) = 0; //CustomTimeDilation Offset
}
else {
    *(float*)(LocalPawn + 0x98) = 1; //CustomTimeDilation Offset
}
NoSpread:

PVOID SpreadCaller = nullptr;
BOOL(*Spread)(PVOID a1, float* a2, float* a3);
BOOL SpreadHook(PVOID a1, float* a2, float* a3)
{
	if (Settings::NoSpread && _ReturnAddress() == SpreadCaller && IsAiming()) {
		return 0; //Spread Value (0 for nospread)
	}

	return Spread(a1, a2, a3);
}

auto addr = MemoryHelper::PatternScan(xorstr("E8 ? ? ? ? 48 8D 4B 28 E8 ? ? ? ? 48 8B C8")); //CalculateSpreadHook Signature
addr = RVA(addr, 5);
DiscordHelper::InsertHook(addr, (uintptr_t)SpreadHook, (uintptr_t)&Spread);

SpreadCaller = (PVOID)(MemoryHelper::PatternScan(xorstr("0F 57 D2 48 8D 4C 24 ? 41 0F 28 CC E8 ? ? ? ? 48 8B 4D B0 0F 28 F0 48 85 C9"))); //SpreadCaller Signature


External
Instant Revive:

write<float>(LocalPawn + 0x3580, 1);
No weapon animation (cba to update):

uintptr_t CurrentWeapon = read<uintptr_t>(LocalPawn + 0x5E8); //CurrentWeapon Offset
if(CurrentWeapon) {
    write<bool>(CurrentWeapon + 0x2B3, true); //bDisableEquipAnimation Offset (update this yourself)
}
RapidFire:

float a = 0;
float b = 0;
uintptr_t CurrentWeapon = read<uintptr_t>(LocalPawn + 0x5E8); //CurrentWeapon Offset
if (CurrentWeapon) {
    a = read<float>(CurrentWeapon + 0x9F4); //LastFireTime Offset
    b = read<float>(CurrentWeapon + 0x9F8); //LastFireTimeVerified Offset
    write<float>(CurrentWeapon + 0x9F4, a + b - 0.0003); //LastFireTime Offset
}
AirStuck:

if (GetAsyncKeyState(VK_MENU)) { //Alt Keybind
    write<float>(LocalPawn + 0x98, 0) //CustomTimeDilation Offset
}
else {
    write<float>(LocalPawn + 0x98, 1); //CustomTimeDilation Offset
}
