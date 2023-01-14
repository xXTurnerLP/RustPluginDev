[[Back]](../README.md)

Rust uses `Unity UI` to render UI components to the client, this is better known as CUI

# Handler
**[Outdated]** Rust (3rd Aug 2019 build) C# decompiled
```cs
// rpc message is
BaseEntity.RPCMessage rpcmessage = new BaseEntity.RPCMessage
{
	connection = msg.connection, // target player's connection object, the player that will see the UI
	player = player, // always null
	read = msg.read // message (json string) containing the Unity UI components
};

// Token: 0x0600069D RID: 1693 RVA: 0x0002F8C8 File Offset: 0x0002DAC8
[BaseEntity.RPC_Client]
public void AddUI(BaseEntity.RPCMessage msg)
{
	string text = msg.read.String();
	if (string.IsNullOrEmpty(text))
	{
		return;
	}
	JSON.Array array = JSON.Array.Parse(text);
	if (array == null)
	{
		return;
	}
	foreach (Value value in array)
	{
		JSON.Object obj = value.Obj;
		GameObject gameObject = this.FindPanel(obj.GetString("parent", "Overlay"));
		if (gameObject == null)
		{
			Debug.LogWarning("AddUI: Unknown Parent for \"" + obj.GetString("name", "AddUI CreatedPanel") + "\": " + obj.GetString("parent", "Overlay"));
			break;
		}
		GameObject gameObject2 = new GameObject(obj.GetString("name", "AddUI CreatedPanel"));
		gameObject2.transform.SetParent(gameObject.transform, false);
		CommunityEntity.RegisterUi(gameObject2);
		RectTransform component = gameObject2.GetComponent<RectTransform>();
		if (component)
		{
			component.anchorMin = new Vector2(0f, 0f);
			component.anchorMax = new Vector2(1f, 1f);
			component.offsetMin = new Vector2(0f, 0f);
			component.offsetMax = new Vector2(1f, 1f);
		}
		foreach (Value value2 in obj.GetArray("components"))
		{
			this.CreateComponents(gameObject2, value2.Obj);
		}
		if (obj.ContainsKey("fadeOut"))
		{
			gameObject2.AddComponent<CommunityEntity.FadeOut>().duration = obj.GetFloat("fadeOut", 0f);
		}
	}
}
```
---
Rust (October 2022 build) C++ decompiled
```cpp
void __stdcall CommunityEntity__AddUI(CommunityEntity_o *this, BaseEntity_RPCMessage_o *msg, const MethodInfo *method)
{
  __int64 v3; // r9
  UnityEngine_Component_o *v5; // r15
  __int64 v6; // rsi
  int v7; // er12
  __int64 v8; // rdx
  __int64 v9; // r8
  __int64 v10; // r9
  ConVar_Demo_c *v11; // rax
  System_String_o *v12; // rbx
  JSON_Array_o *v13; // rax
  System_Collections_Generic_IEnumerator_Value__o *v14; // rdi
  __int64 v15; // rbx
  __int64 v16; // rax
  JSON_Object_o *v17; // r13
  __int64 v18; // rdx
  System_String_o *v19; // rbx
  __int64 v20; // r8
  __int64 v21; // r9
  CommunityEntity_c *v22; // rax
  System_Collections_Generic_Dictionary_WeatherPresetType__WeatherPreset____o *v23; // rcx
  __int64 v24; // rdx
  __int64 v25; // r8
  __int64 v26; // r9
  UnityEngine_Transform_o *v27; // rax
  __int64 v28; // rdx
  UnityEngine_Component_o *v29; // rbx
  __int64 v30; // r8
  __int64 v31; // r9
  UnityEngine_Object_o *v32; // rsi
  System_String_o *v33; // r14
  __int64 v34; // rdx
  System_Type_array *v35; // rdi
  __int64 v36; // r8
  __int64 v37; // r9
  Il2CppType *v38; // rbx
  System_Type_o *v39; // rax
  __int64 v40; // rbx
  UnityEngine_GameObject_o *v41; // r15
  UnityEngine_Transform_o *v42; // rbx
  UnityEngine_Transform_o *v43; // rax
  __int64 v44; // rdx
  __int64 v45; // r8
  __int64 v46; // r9
  __int64 v47; // rdx
  Il2CppObject *v48; // rbx
  __int64 v49; // r8
  __int64 v50; // r9
  __int64 v51; // rdx
  __int64 v52; // r8
  __int64 v53; // rdx
  __int64 v54; // r8
  __int64 v55; // rdx
  __int64 v56; // r8
  __int64 v57; // rdx
  __int64 v58; // r8
  JSON_Array_o *v59; // rax
  System_Collections_Generic_IEnumerator_Value__o *v60; // rbx
  __int64 v61; // rax
  int v62; // edi
  bool v63; // al
  Il2CppObject *v64; // rbx
  float v65; // xmm0_4
  System_String_o *v66; // rbx
  System_String_o *v67; // rax
  __int64 v68; // rdx
  Il2CppObject *v69; // rbx
  __int64 v70; // r8
  __int64 v71; // r9
  int v72; // er12
  __int64 v73; // r13
  __int64 v74; // rax
  __int64 v75; // rax
  char v76[16]; // [rsp+20h] [rbp-10h] BYREF
  __int64 v77; // [rsp+30h] [rbp+0h]
  int v78; // [rsp+38h] [rbp+8h]
  char *v79; // [rsp+40h] [rbp+10h]
  UnityEngine_Vector2_o v80; // [rsp+48h] [rbp+18h] BYREF
  System_Collections_Generic_IEnumerator_Value__o *v81; // [rsp+50h] [rbp+20h]
  WeatherPreset_array *value; // [rsp+58h] [rbp+28h] BYREF
  UnityEngine_Vector2_o v83; // [rsp+60h] [rbp+30h] BYREF
  UnityEngine_Vector2_o v84; // [rsp+68h] [rbp+38h] BYREF
  UnityEngine_Vector2_o v85; // [rsp+70h] [rbp+40h] BYREF
  char *v86; // [rsp+78h] [rbp+48h]
  JSON_Object_o *v87; // [rsp+80h] [rbp+50h]
  UnityEngine_GameObject_o *v88; // [rsp+88h] [rbp+58h]
  Network_NetRead_o *v89; // [rsp+B0h] [rbp+80h]
  CommunityEntity_o *v90; // [rsp+120h] [rbp+F0h]

  v90 = this;
  v5 = (UnityEngine_Component_o *)this;
  if ( !byte_1832F47E4 )
  {
    sub_1803B9870(14897i64, (__int64)msg);
    byte_1832F47E4 = 1;
  }
  v6 = 0i64;
  v77 = 0i64;
  v79 = v76;
  v86 = v76;
  v7 = -1;
  if ( (Client_TypeInfo->_2.bitflags2 & 2) != 0 && !Client_TypeInfo->_2.cctor_finished )
    il2cpp_runtime_class_init(Client_TypeInfo, msg, method, v3);
  if ( !Client__get_IsPlayingDemo(0i64) )
    goto LABEL_103;
  v11 = ConVar_Demo_TypeInfo;
  if ( (ConVar_Demo_TypeInfo->_2.bitflags2 & 2) != 0 && !ConVar_Demo_TypeInfo->_2.cctor_finished )
  {
    il2cpp_runtime_class_init(ConVar_Demo_TypeInfo, v8, v9, v10);
    v11 = ConVar_Demo_TypeInfo;
  }
  if ( v11->static_fields->showCommunityUI )
  {
LABEL_103:
    v89 = msg->fields.read;
    if ( !v89 )
      null_ref_exception();
    v12 = Network_NetRead__StringRaw(v89, 0x800000u, 0i64);
    if ( !System_Net_ValidationHelper__IsBlankString(v12, 0i64) )
    {
      v13 = JSON_Array__Parse(v12, 0i64);
      if ( v13 )
      {
        v14 = JSON_Array__GetEnumerator(v13, 0i64);
        v81 = v14;
        while ( 1 )
        {
          v15 = v7;
          v78 = v7;
          if ( !v14 )
            null_ref_exception();
          if ( !(unsigned __int8)sub_1800F79C0(0i64, System_Collections_IEnumerator_TypeInfo) )
          {
            v72 = v7 + 1;
            v73 = (__int64)v79;
            *(_DWORD *)&v79[4 * v15 + 4] = 473;
            goto LABEL_78;
          }
          v16 = sub_1800F79C0(0i64, System_Collections_Generic_IEnumerator_Value__TypeInfo);
          if ( !v16 )
            null_ref_exception();
          v17 = *(JSON_Object_o **)(v16 + 40);
          v87 = v17;
          if ( !v17 )
            null_ref_exception();
          v19 = JSON_Object__GetString(v17, StringLiteral_3352, StringLiteral_16416, 0i64);
          if ( !byte_1832F47E5 )
          {
            sub_1803B9870(14905i64, v18);
            byte_1832F47E5 = 1;
          }
          value = 0i64;
          v22 = CommunityEntity_TypeInfo;
          if ( (CommunityEntity_TypeInfo->_2.bitflags2 & 2) != 0 && !CommunityEntity_TypeInfo->_2.cctor_finished )
          {
            il2cpp_runtime_class_init(CommunityEntity_TypeInfo, v18, v20, v21);
            v22 = CommunityEntity_TypeInfo;
          }
          v23 = (System_Collections_Generic_Dictionary_WeatherPresetType__WeatherPreset____o *)v22->static_fields->UiDict;
          if ( !v23 )
            null_ref_exception();
          if ( System_Collections_Generic_Dictionary_WeatherPresetType__WeatherPreset_____TryGetValue(
                 v23,
                 (int32_t)v19,
                 &value,
                 Method_System_Collections_Generic_Dictionary_string__GameObject__TryGetValue__) )
          {
            v32 = (UnityEngine_Object_o *)value;
          }
          else
          {
            v27 = UnityEngine_Component__get_transform(v5, 0i64);
            v29 = (UnityEngine_Component_o *)Facepunch_Extend_TransformEx__FindChildRecursive(v27, v19, 0i64);
            if ( (UnityEngine_Object_TypeInfo->_2.bitflags2 & 2) != 0 && !UnityEngine_Object_TypeInfo->_2.cctor_finished )
              il2cpp_runtime_class_init(UnityEngine_Object_TypeInfo, v28, v30, v31);
            if ( UnityEngine_Object__op_Implicit((UnityEngine_Object_o *)v29, 0i64) )
            {
              if ( !v29 )
                null_ref_exception();
              v32 = (UnityEngine_Object_o *)UnityEngine_Component__get_gameObject(v29, 0i64);
            }
            else
            {
              v32 = 0i64;
            }
          }
          if ( (UnityEngine_Object_TypeInfo->_2.bitflags2 & 2) != 0 && !UnityEngine_Object_TypeInfo->_2.cctor_finished )
            il2cpp_runtime_class_init(UnityEngine_Object_TypeInfo, v24, v25, v26);
          if ( UnityEngine_Object__op_Equality(v32, 0i64, 0i64) )
            break;
          v33 = JSON_Object__GetString(v17, StringLiteral_6, StringLiteral_16418, 0i64);
          v35 = (System_Type_array *)sub_1803B9650(System_Type___TypeInfo, 1i64);
          v38 = UnityEngine_RectTransform_var;
          if ( (System_Type_TypeInfo->_2.bitflags2 & 2) != 0 && !System_Type_TypeInfo->_2.cctor_finished )
            il2cpp_runtime_class_init(System_Type_TypeInfo, v34, v36, v37);
          v39 = System_Type__GetTypeFromHandle((System_RuntimeTypeHandle_o)v38, 0i64);
          v40 = (__int64)v39;
          if ( !v35 )
            null_ref_exception();
          if ( v39 && !sub_1803B95E0(v39, v35->obj.klass->_1.element_class) )
          {
            v74 = sub_1803B97C0();
            sub_1803B99B0(v74, 0i64);
          }
          if ( !LODWORD(v35->max_length) )
          {
            v75 = sub_1803B97F0();
            sub_1803B99B0(v75, 0i64);
          }
          v35->m_Items[0] = (System_Type_o *)v40;
          init_csharp_struct_member((__int64)v35->m_Items, v40);
          v41 = (UnityEngine_GameObject_o *)sub_1803B99A0(UnityEngine_GameObject_TypeInfo);
          UnityEngine_GameObject___ctor_6470564944(v41, v33, v35, 0i64);
          v88 = v41;
          if ( !v41 )
            null_ref_exception();
          v42 = UnityEngine_GameObject__get_transform(v41, 0i64);
          if ( !v32 )
            null_ref_exception();
          v43 = UnityEngine_GameObject__get_transform((UnityEngine_GameObject_o *)v32, 0i64);
          if ( !v42 )
            null_ref_exception();
          UnityEngine_Transform__SetParent_6480146336(v42, v43, 0, 0i64);
          if ( (CommunityEntity_TypeInfo->_2.bitflags2 & 2) != 0 && !CommunityEntity_TypeInfo->_2.cctor_finished )
            il2cpp_runtime_class_init(CommunityEntity_TypeInfo, v44, v45, v46);
          CommunityEntity__RegisterUi(v41, 0i64);
          v48 = UnityEngine_GameObject__GetComponent_object_(
                  v41,
                  Method_UnityEngine_GameObject_GetComponent_RectTransform___);
          if ( (UnityEngine_Object_TypeInfo->_2.bitflags2 & 2) != 0 && !UnityEngine_Object_TypeInfo->_2.cctor_finished )
            il2cpp_runtime_class_init(UnityEngine_Object_TypeInfo, v47, v49, v50);
          if ( UnityEngine_Object__op_Implicit((UnityEngine_Object_o *)v48, 0i64) )
          {
            v80 = 0i64;
            sub_1811F0DD0(&v80, v51, v52, 0i64);
            if ( !v48 )
              null_ref_exception();
            UnityEngine_RectTransform__set_anchorMin((UnityEngine_RectTransform_o *)v48, v80, 0i64);
            v83 = 0i64;
            sub_1811F0DD0(&v83, v53, v54, 0i64);
            UnityEngine_RectTransform__set_anchorMax((UnityEngine_RectTransform_o *)v48, v83, 0i64);
            v84 = 0i64;
            sub_1811F0DD0(&v84, v55, v56, 0i64);
            UnityEngine_RectTransform__set_offsetMin((UnityEngine_RectTransform_o *)v48, v84, 0i64);
            v85 = 0i64;
            sub_1811F0DD0(&v85, v57, v58, 0i64);
            UnityEngine_RectTransform__set_offsetMax((UnityEngine_RectTransform_o *)v48, v85, 0i64);
          }
          v59 = JSON_Object__GetArray(v17, StringLiteral_5982, 0i64);
          if ( !v59 )
            null_ref_exception();
          v60 = JSON_Array__GetEnumerator(v59, 0i64);
          v80 = (UnityEngine_Vector2_o)v60;
          while ( 1 )
          {
            if ( !v60 )
              null_ref_exception();
            if ( !(unsigned __int8)sub_1800F79C0(0i64, System_Collections_IEnumerator_TypeInfo) )
              break;
            v61 = sub_1800F79C0(0i64, System_Collections_Generic_IEnumerator_Value__TypeInfo);
            if ( !v61 )
              null_ref_exception();
            CommunityEntity__CreateComponents(v90, v41, *(JSON_Object_o **)(v61 + 40), 0i64);
          }
          ++v7;
          *(_DWORD *)&v79[4 * v78 + 4] = 409;
          v6 = v77;
          v62 = v7;
          sub_1800F79C0(0i64, System_IDisposable_TypeInfo);
          if ( v7 == -1 || *(_DWORD *)&v79[4 * v7] != 409 )
          {
            if ( v6 )
              sub_1803B99B0(v6, 0i64);
          }
          else
          {
            --v7;
            if ( v62 < 0 )
              v7 = v62;
          }
          v63 = JSON_Object__ContainsKey(v17, StringLiteral_16420, 0i64);
          v14 = v81;
          if ( v63 )
          {
            v64 = UnityEngine_GameObject__AddComponent_object_(
                    v41,
                    Method_UnityEngine_GameObject_AddComponent_CommunityEntity_FadeOut___);
            v65 = JSON_Object__GetFloat(v17, StringLiteral_16420, 0.0, 0i64);
            if ( !v64 )
              null_ref_exception();
            *(float *)&v64[1].monitor = v65;
          }
          v5 = (UnityEngine_Component_o *)v90;
        }
        v66 = JSON_Object__GetString(v17, StringLiteral_6, StringLiteral_16418, 0i64);
        v67 = JSON_Object__GetString(v17, StringLiteral_3352, StringLiteral_16416, 0i64);
        v69 = (Il2CppObject *)System_String__Concat_6470865648(StringLiteral_16417, v66, StringLiteral_16419, v67, 0i64);
        if ( (UnityEngine_Debug_TypeInfo->_2.bitflags2 & 2) != 0 && !UnityEngine_Debug_TypeInfo->_2.cctor_finished )
          il2cpp_runtime_class_init(UnityEngine_Debug_TypeInfo, v68, v70, v71);
        UnityEngine_Debug__LogWarning(v69, 0i64);
        v72 = v7 + 1;
        v73 = (__int64)v79;
        *(_DWORD *)&v79[4 * v78 + 4] = 473;
        v6 = v77;
LABEL_78:
        sub_1800F79C0(0i64, System_IDisposable_TypeInfo);
        if ( (v72 == -1 || *(_DWORD *)(v73 + 4i64 * v72) != 473) && v6 )
          sub_1803B99B0(v6, 0i64);
      }
    }
  }
}
```
---
Rust (October 2022 build) C++ disassembled
```nasm
il2cpp:00000001806629B0 ; =============== S U B R O U T I N E =======================================
il2cpp:00000001806629B0
il2cpp:00000001806629B0 ; Attributes: bp-based frame fpd=0E0h
il2cpp:00000001806629B0
il2cpp:00000001806629B0 ; void __stdcall CommunityEntity__AddUI(CommunityEntity_o *this, BaseEntity_RPCMessage_o msg, const MethodInfo *method)
il2cpp:00000001806629B0 CommunityEntity$$AddUI proc near        ; CODE XREF: CommunityEntity$$OnRpcMessage+1BE↓p
il2cpp:00000001806629B0                                         ; DATA XREF: .data:00000001831D1F90↓o ...
il2cpp:00000001806629B0
il2cpp:00000001806629B0 var_110         = dword ptr -110h
il2cpp:00000001806629B0 method          = qword ptr -100h
il2cpp:00000001806629B0 var_F0          = byte ptr -0F0h
il2cpp:00000001806629B0 var_E0          = qword ptr -0E0h
il2cpp:00000001806629B0 var_D8          = dword ptr -0D8h
il2cpp:00000001806629B0 var_D0          = qword ptr -0D0h
il2cpp:00000001806629B0 var_C8          = UnityEngine_Vector2_o ptr -0C8h
il2cpp:00000001806629B0 var_C0          = qword ptr -0C0h
il2cpp:00000001806629B0 value           = qword ptr -0B8h
il2cpp:00000001806629B0 var_B0          = UnityEngine_Vector2_o ptr -0B0h
il2cpp:00000001806629B0 var_A8          = UnityEngine_Vector2_o ptr -0A8h
il2cpp:00000001806629B0 var_A0          = UnityEngine_Vector2_o ptr -0A0h
il2cpp:00000001806629B0 var_98          = qword ptr -98h
il2cpp:00000001806629B0 var_90          = qword ptr -90h
il2cpp:00000001806629B0 var_88          = qword ptr -88h
il2cpp:00000001806629B0 var_60          = qword ptr -60h
il2cpp:00000001806629B0 var_50          = xmmword ptr -50h
il2cpp:00000001806629B0 var_40          = xmmword ptr -40h
il2cpp:00000001806629B0 arg_0           = qword ptr  10h
il2cpp:00000001806629B0 arg_8           = qword ptr  18h
il2cpp:00000001806629B0 arg_18          = dword ptr  28h
il2cpp:00000001806629B0
il2cpp:00000001806629B0 ; __unwind { // __CxxFrameHandler4
il2cpp:00000001806629B0                 mov     [rsp-8+arg_0], rcx
il2cpp:00000001806629B5                 push    rbp
il2cpp:00000001806629B6                 push    rsi
il2cpp:00000001806629B7                 push    rdi
il2cpp:00000001806629B8                 push    r12
il2cpp:00000001806629BA                 push    r13
il2cpp:00000001806629BC                 push    r14
il2cpp:00000001806629BE                 push    r15
il2cpp:00000001806629C0                 sub     rsp, 0E0h
il2cpp:00000001806629C7                 lea     rbp, [rsp+30h]
il2cpp:00000001806629CC                 mov     [rbp+0E0h+arg_8], rbx
il2cpp:00000001806629D3                 movaps  [rbp+0E0h+var_40], xmm6
il2cpp:00000001806629DA                 movaps  [rbp+0E0h+var_50], xmm7
il2cpp:00000001806629E1                 mov     rbx, rdx
il2cpp:00000001806629E4                 mov     r15, rcx
il2cpp:00000001806629E7                 cmp     cs:byte_1832F47E4, 0
il2cpp:00000001806629EE                 jnz     short loc_180662A02
il2cpp:00000001806629F0                 mov     ecx, cs:dword_1828145B0
il2cpp:00000001806629F6                 call    sub_1803B9870
il2cpp:00000001806629FB                 mov     cs:byte_1832F47E4, 1
il2cpp:0000000180662A02
il2cpp:0000000180662A02 loc_180662A02:                          ; CODE XREF: CommunityEntity$$AddUI+3E↑j
il2cpp:0000000180662A02                 xor     r14d, r14d
il2cpp:0000000180662A05                 mov     esi, r14d
il2cpp:0000000180662A08                 mov     [rbp+0E0h+var_E0], r14
il2cpp:0000000180662A0C                 mov     eax, [rsp+110h+var_110]
il2cpp:0000000180662A0F                 sub     rsp, 10h
il2cpp:0000000180662A13                 lea     r13, [rsp+120h+var_F0]
il2cpp:0000000180662A18                 mov     [rbp+0E0h+var_D0], r13
il2cpp:0000000180662A1C                 mov     eax, [r13+0]
il2cpp:0000000180662A20                 mov     [rbp+0E0h+var_98], r13
il2cpp:0000000180662A24                 mov     r12d, 0FFFFFFFFh
il2cpp:0000000180662A2A                 mov     [rbp+0E0h+arg_18], r12d
il2cpp:0000000180662A31                 mov     rcx, cs:Client_TypeInfo ; Client_TypeInfo
il2cpp:0000000180662A38                 test    byte ptr [rcx+12Fh], 2
il2cpp:0000000180662A3F                 jz      short loc_180662A4E
il2cpp:0000000180662A41                 cmp     [rcx+0E0h], esi
il2cpp:0000000180662A47                 jnz     short loc_180662A4E
il2cpp:0000000180662A49                 call    il2cpp_runtime_class_init
il2cpp:0000000180662A4E
il2cpp:0000000180662A4E loc_180662A4E:                          ; CODE XREF: CommunityEntity$$AddUI+8F↑j
il2cpp:0000000180662A4E                                         ; CommunityEntity$$AddUI+97↑j
il2cpp:0000000180662A4E                 xor     ecx, ecx        ; method
il2cpp:0000000180662A50                 call    Client$$get_IsPlayingDemo
il2cpp:0000000180662A55                 test    al, al
il2cpp:0000000180662A57                 jz      short loc_180662A91
il2cpp:0000000180662A59                 mov     rax, cs:ConVar_Demo_TypeInfo ; ConVar.Demo_TypeInfo
il2cpp:0000000180662A60                 test    byte ptr [rax+12Fh], 2
il2cpp:0000000180662A67                 jz      short loc_180662A80
il2cpp:0000000180662A69                 cmp     [rax+0E0h], esi
il2cpp:0000000180662A6F                 jnz     short loc_180662A80
il2cpp:0000000180662A71                 mov     rcx, rax
il2cpp:0000000180662A74                 call    il2cpp_runtime_class_init
il2cpp:0000000180662A79                 mov     rax, cs:ConVar_Demo_TypeInfo ; ConVar.Demo_TypeInfo
il2cpp:0000000180662A80
il2cpp:0000000180662A80 loc_180662A80:                          ; CODE XREF: CommunityEntity$$AddUI+B7↑j
il2cpp:0000000180662A80                                         ; CommunityEntity$$AddUI+BF↑j
il2cpp:0000000180662A80                 mov     rax, [rax+0B8h]
il2cpp:0000000180662A87                 cmp     [rax+10h], sil
il2cpp:0000000180662A8B                 jz      loc_180663109
il2cpp:0000000180662A91
il2cpp:0000000180662A91 loc_180662A91:                          ; CODE XREF: CommunityEntity$$AddUI+A7↑j
il2cpp:0000000180662A91                 movsd   xmm1, qword ptr [rbx+10h]
il2cpp:0000000180662A96                 movsd   [rbp+0E0h+var_60], xmm1
il2cpp:0000000180662A9E                 mov     rcx, [rbp+0E0h+var_60] ; this
il2cpp:0000000180662AA5                 test    rcx, rcx
il2cpp:0000000180662AA8                 jz      loc_1806631AE
il2cpp:0000000180662AAE                 xor     r8d, r8d        ; method
il2cpp:0000000180662AB1                 mov     edx, 800000h    ; maxLength
il2cpp:0000000180662AB6                 call    Network_NetRead$$StringRaw
il2cpp:0000000180662ABB                 mov     rbx, rax
il2cpp:0000000180662ABE                 xor     edx, edx        ; method
il2cpp:0000000180662AC0                 mov     rcx, rax        ; stringValue
il2cpp:0000000180662AC3                 call    System_Net_ValidationHelper$$IsBlankString
il2cpp:0000000180662AC8                 test    al, al
il2cpp:0000000180662ACA                 jnz     loc_180663109
il2cpp:0000000180662AD0                 xor     edx, edx        ; method
il2cpp:0000000180662AD2                 mov     rcx, rbx        ; jsonString
il2cpp:0000000180662AD5                 call    JSON_Array$$Parse
il2cpp:0000000180662ADA                 test    rax, rax
il2cpp:0000000180662ADD                 jz      loc_180663109
il2cpp:0000000180662AE3                 xor     edx, edx        ; method
il2cpp:0000000180662AE5                 mov     rcx, rax        ; this
il2cpp:0000000180662AE8                 call    JSON_Array$$GetEnumerator
il2cpp:0000000180662AED                 mov     rdi, rax
il2cpp:0000000180662AF0                 mov     [rbp+0E0h+var_C0], rax
il2cpp:0000000180662AF4                 xorps   xmm6, xmm6
il2cpp:0000000180662AF7                 movss   xmm7, cs:valueEnd
il2cpp:0000000180662AFF                 jmp     short loc_180662B17
il2cpp:0000000180662AFF ; ---------------------------------------------------------------------------
il2cpp:0000000180662B01                 align 10h
il2cpp:0000000180662B10
il2cpp:0000000180662B10 loc_180662B10:                          ; CODE XREF: CommunityEntity$$AddUI+624↓j
il2cpp:0000000180662B10                                         ; CommunityEntity$$AddUI+65F↓j
il2cpp:0000000180662B10                 mov     r15, [rbp+0E0h+arg_0]
il2cpp:0000000180662B17
il2cpp:0000000180662B17 loc_180662B17:                          ; CODE XREF: CommunityEntity$$AddUI+14F↑j
il2cpp:0000000180662B17                 movsxd  rbx, r12d
il2cpp:0000000180662B1A                 mov     [rbp+0E0h+var_D8], ebx
il2cpp:0000000180662B1D                 test    rdi, rdi
il2cpp:0000000180662B20                 jz      loc_18066319D
il2cpp:0000000180662B26                 mov     r8, rdi
il2cpp:0000000180662B29                 mov     rdx, cs:System_Collections_IEnumerator_TypeInfo ; System.Collections.IEnumerator_TypeInfo
il2cpp:0000000180662B30                 xor     ecx, ecx
il2cpp:0000000180662B32                 call    sub_1800F79C0
il2cpp:0000000180662B37                 test    al, al
il2cpp:0000000180662B39                 jz      loc_1806630AA
il2cpp:0000000180662B3F                 mov     r8, rdi
il2cpp:0000000180662B42                 mov     rdx, cs:System_Collections_Generic_IEnumerator_Value__TypeInfo ; System.Collections.Generic.IEnumerator<Value>_TypeInfo
il2cpp:0000000180662B49                 xor     ecx, ecx
il2cpp:0000000180662B4B                 call    sub_1800F79C0
il2cpp:0000000180662B50                 test    rax, rax
il2cpp:0000000180662B53                 jz      loc_180663198
il2cpp:0000000180662B59                 mov     r13, [rax+28h]
il2cpp:0000000180662B5D                 mov     [rbp+0E0h+var_90], r13
il2cpp:0000000180662B61                 test    r13, r13
il2cpp:0000000180662B64                 jz      loc_180663193
il2cpp:0000000180662B6A                 xor     r9d, r9d        ; method
il2cpp:0000000180662B6D                 mov     r8, cs:StringLiteral_16416 ; strDEFAULT
il2cpp:0000000180662B74                 mov     rdx, cs:StringLiteral_3352 ; key
il2cpp:0000000180662B7B                 mov     rcx, r13        ; this
il2cpp:0000000180662B7E                 call    JSON_Object$$GetString
il2cpp:0000000180662B83                 mov     rbx, rax
il2cpp:0000000180662B86                 cmp     cs:byte_1832F47E5, 0
il2cpp:0000000180662B8D                 jnz     short loc_180662BA1
il2cpp:0000000180662B8F                 mov     ecx, cs:dword_1828146CC
il2cpp:0000000180662B95                 call    sub_1803B9870
il2cpp:0000000180662B9A                 mov     cs:byte_1832F47E5, 1
il2cpp:0000000180662BA1
il2cpp:0000000180662BA1 loc_180662BA1:                          ; CODE XREF: CommunityEntity$$AddUI+1DD↑j
il2cpp:0000000180662BA1                 mov     [rbp+0E0h+value], r14
il2cpp:0000000180662BA5                 mov     rax, cs:CommunityEntity_TypeInfo ; CommunityEntity_TypeInfo
il2cpp:0000000180662BAC                 test    byte ptr [rax+12Fh], 2
il2cpp:0000000180662BB3                 jz      short loc_180662BCD
il2cpp:0000000180662BB5                 cmp     dword ptr [rax+0E0h], 0
il2cpp:0000000180662BBC                 jnz     short loc_180662BCD
il2cpp:0000000180662BBE                 mov     rcx, rax
il2cpp:0000000180662BC1                 call    il2cpp_runtime_class_init
il2cpp:0000000180662BC6                 mov     rax, cs:CommunityEntity_TypeInfo ; CommunityEntity_TypeInfo
il2cpp:0000000180662BCD
il2cpp:0000000180662BCD loc_180662BCD:                          ; CODE XREF: CommunityEntity$$AddUI+203↑j
il2cpp:0000000180662BCD                                         ; CommunityEntity$$AddUI+20C↑j
il2cpp:0000000180662BCD                 mov     rax, [rax+0B8h]
il2cpp:0000000180662BD4                 mov     rcx, [rax+18h]  ; this
il2cpp:0000000180662BD8                 test    rcx, rcx
il2cpp:0000000180662BDB                 jz      loc_18066318E
il2cpp:0000000180662BE1                 mov     r9, cs:Method$System_Collections_Generic_Dictionary_string__GameObject__TryGetValue__ ; method
il2cpp:0000000180662BE8                 lea     r8, [rbp+0E0h+value] ; value
il2cpp:0000000180662BEC                 mov     rdx, rbx        ; key
il2cpp:0000000180662BEF                 call    System_Collections_Generic_Dictionary_WeatherPresetType__WeatherPreset___$$TryGetValue
il2cpp:0000000180662BF4                 test    al, al
il2cpp:0000000180662BF6                 jnz     short loc_180662C5C
il2cpp:0000000180662BF8                 xor     edx, edx        ; method
il2cpp:0000000180662BFA                 mov     rcx, r15        ; this
il2cpp:0000000180662BFD                 call    UnityEngine_Component$$get_transform
il2cpp:0000000180662C02                 xor     r8d, r8d        ; method
il2cpp:0000000180662C05                 mov     rdx, rbx        ; name
il2cpp:0000000180662C08                 mov     rcx, rax        ; transform
il2cpp:0000000180662C0B                 call    Facepunch_Extend_TransformEx$$FindChildRecursive
il2cpp:0000000180662C10                 mov     rbx, rax
il2cpp:0000000180662C13                 mov     rcx, cs:UnityEngine_Object_TypeInfo ; UnityEngine.Object_TypeInfo
il2cpp:0000000180662C1A                 test    byte ptr [rcx+12Fh], 2
il2cpp:0000000180662C21                 jz      short loc_180662C31
il2cpp:0000000180662C23                 cmp     dword ptr [rcx+0E0h], 0
il2cpp:0000000180662C2A                 jnz     short loc_180662C31
il2cpp:0000000180662C2C                 call    il2cpp_runtime_class_init
il2cpp:0000000180662C31
il2cpp:0000000180662C31 loc_180662C31:                          ; CODE XREF: CommunityEntity$$AddUI+271↑j
il2cpp:0000000180662C31                                         ; CommunityEntity$$AddUI+27A↑j
il2cpp:0000000180662C31                 xor     edx, edx        ; method
il2cpp:0000000180662C33                 mov     rcx, rbx        ; exists
il2cpp:0000000180662C36                 call    UnityEngine_Object$$op_Implicit
il2cpp:0000000180662C3B                 test    al, al
il2cpp:0000000180662C3D                 jnz     short loc_180662C44
il2cpp:0000000180662C3F                 mov     rsi, r14
il2cpp:0000000180662C42                 jmp     short loc_180662C60
il2cpp:0000000180662C44 ; ---------------------------------------------------------------------------
il2cpp:0000000180662C44
il2cpp:0000000180662C44 loc_180662C44:                          ; CODE XREF: CommunityEntity$$AddUI+28D↑j
il2cpp:0000000180662C44                 test    rbx, rbx
il2cpp:0000000180662C47                 jz      loc_180663131
il2cpp:0000000180662C4D                 xor     edx, edx        ; method
il2cpp:0000000180662C4F                 mov     rcx, rbx        ; this
il2cpp:0000000180662C52                 call    UnityEngine_Component$$get_gameObject
il2cpp:0000000180662C57                 mov     rsi, rax
il2cpp:0000000180662C5A                 jmp     short loc_180662C60
il2cpp:0000000180662C5C ; ---------------------------------------------------------------------------
il2cpp:0000000180662C5C
il2cpp:0000000180662C5C loc_180662C5C:                          ; CODE XREF: CommunityEntity$$AddUI+246↑j
il2cpp:0000000180662C5C                 mov     rsi, [rbp+0E0h+value]
il2cpp:0000000180662C60
il2cpp:0000000180662C60 loc_180662C60:                          ; CODE XREF: CommunityEntity$$AddUI+292↑j
il2cpp:0000000180662C60                                         ; CommunityEntity$$AddUI+2AA↑j
il2cpp:0000000180662C60                 mov     rcx, cs:UnityEngine_Object_TypeInfo ; UnityEngine.Object_TypeInfo
il2cpp:0000000180662C67                 test    byte ptr [rcx+12Fh], 2
il2cpp:0000000180662C6E                 jz      short loc_180662C7E
il2cpp:0000000180662C70                 cmp     dword ptr [rcx+0E0h], 0
il2cpp:0000000180662C77                 jnz     short loc_180662C7E
il2cpp:0000000180662C79                 call    il2cpp_runtime_class_init
il2cpp:0000000180662C7E
il2cpp:0000000180662C7E loc_180662C7E:                          ; CODE XREF: CommunityEntity$$AddUI+2BE↑j
il2cpp:0000000180662C7E                                         ; CommunityEntity$$AddUI+2C7↑j
il2cpp:0000000180662C7E                 xor     r8d, r8d        ; method
il2cpp:0000000180662C81                 xor     edx, edx        ; y
il2cpp:0000000180662C83                 mov     rcx, rsi        ; x
il2cpp:0000000180662C86                 call    UnityEngine_Object$$op_Equality
il2cpp:0000000180662C8B                 xor     r9d, r9d        ; method
il2cpp:0000000180662C8E                 mov     r8, cs:StringLiteral_16418 ; strDEFAULT
il2cpp:0000000180662C95                 mov     rdx, cs:StringLiteral_6 ; key
il2cpp:0000000180662C9C                 mov     rcx, r13        ; this
il2cpp:0000000180662C9F                 test    al, al
il2cpp:0000000180662CA1                 jnz     loc_18066301F
il2cpp:0000000180662CA7                 call    JSON_Object$$GetString
il2cpp:0000000180662CAC                 mov     r14, rax
il2cpp:0000000180662CAF                 mov     edx, 1
il2cpp:0000000180662CB4                 mov     rcx, cs:System_Type___TypeInfo ; System.Type[]_TypeInfo
il2cpp:0000000180662CBB                 call    sub_1803B9650
il2cpp:0000000180662CC0                 mov     rdi, rax
il2cpp:0000000180662CC3                 mov     rbx, cs:UnityEngine_RectTransform_var ; UnityEngine.RectTransform_var
il2cpp:0000000180662CCA                 mov     rcx, cs:System_Type_TypeInfo ; System.Type_TypeInfo
il2cpp:0000000180662CD1                 test    byte ptr [rcx+12Fh], 2
il2cpp:0000000180662CD8                 jz      short loc_180662CE8
il2cpp:0000000180662CDA                 cmp     dword ptr [rcx+0E0h], 0
il2cpp:0000000180662CE1                 jnz     short loc_180662CE8
il2cpp:0000000180662CE3                 call    il2cpp_runtime_class_init
il2cpp:0000000180662CE8
il2cpp:0000000180662CE8 loc_180662CE8:                          ; CODE XREF: CommunityEntity$$AddUI+328↑j
il2cpp:0000000180662CE8                                         ; CommunityEntity$$AddUI+331↑j
il2cpp:0000000180662CE8                 xor     edx, edx        ; method
il2cpp:0000000180662CEA                 mov     rcx, rbx        ; handle
il2cpp:0000000180662CED                 call    System_Type$$GetTypeFromHandle
il2cpp:0000000180662CF2                 mov     rbx, rax
il2cpp:0000000180662CF5                 test    rdi, rdi
il2cpp:0000000180662CF8                 jz      loc_180663189
il2cpp:0000000180662CFE                 test    rax, rax
il2cpp:0000000180662D01                 jz      short loc_180662D1B
il2cpp:0000000180662D03                 mov     rdx, [rdi]
il2cpp:0000000180662D06                 mov     rdx, [rdx+40h]
il2cpp:0000000180662D0A                 mov     rcx, rax
il2cpp:0000000180662D0D                 call    sub_1803B95E0
il2cpp:0000000180662D12                 test    rax, rax
il2cpp:0000000180662D15                 jz      loc_180663137
il2cpp:0000000180662D1B
il2cpp:0000000180662D1B loc_180662D1B:                          ; CODE XREF: CommunityEntity$$AddUI+351↑j
il2cpp:0000000180662D1B                 cmp     dword ptr [rdi+18h], 0
il2cpp:0000000180662D1F                 jbe     loc_180663146
il2cpp:0000000180662D25                 lea     rcx, [rdi+20h]
il2cpp:0000000180662D29                 mov     [rcx], rbx
il2cpp:0000000180662D2C                 mov     rdx, rbx
il2cpp:0000000180662D2F                 call    init_csharp_struct_member
il2cpp:0000000180662D34                 mov     rcx, cs:UnityEngine_GameObject_TypeInfo ; UnityEngine.GameObject_TypeInfo
il2cpp:0000000180662D3B                 call    sub_1803B99A0
il2cpp:0000000180662D40                 mov     r15, rax
il2cpp:0000000180662D43                 xor     r9d, r9d        ; method
il2cpp:0000000180662D46                 mov     r8, rdi         ; components
il2cpp:0000000180662D49                 mov     rdx, r14        ; name
il2cpp:0000000180662D4C                 mov     rcx, rax        ; this
il2cpp:0000000180662D4F                 call    UnityEngine_GameObject$$_ctor_6470564944
il2cpp:0000000180662D54                 mov     [rbp+0E0h+var_88], r15
il2cpp:0000000180662D58                 test    r15, r15
il2cpp:0000000180662D5B                 jz      loc_180663184
il2cpp:0000000180662D61                 xor     edx, edx        ; method
il2cpp:0000000180662D63                 mov     rcx, r15        ; this
il2cpp:0000000180662D66                 call    UnityEngine_GameObject$$get_transform
il2cpp:0000000180662D6B                 mov     rbx, rax
il2cpp:0000000180662D6E                 test    rsi, rsi
il2cpp:0000000180662D71                 jz      loc_18066317F
il2cpp:0000000180662D77                 xor     edx, edx        ; method
il2cpp:0000000180662D79                 mov     rcx, rsi        ; this
il2cpp:0000000180662D7C                 call    UnityEngine_GameObject$$get_transform
il2cpp:0000000180662D81                 test    rbx, rbx
il2cpp:0000000180662D84                 jz      loc_18066317A
il2cpp:0000000180662D8A                 xor     r9d, r9d        ; method
il2cpp:0000000180662D8D                 xor     r8d, r8d        ; worldPositionStays
il2cpp:0000000180662D90                 mov     rdx, rax        ; parent
il2cpp:0000000180662D93                 mov     rcx, rbx        ; this
il2cpp:0000000180662D96                 call    UnityEngine_Transform$$SetParent_6480146336
il2cpp:0000000180662D9B                 mov     rcx, cs:CommunityEntity_TypeInfo ; CommunityEntity_TypeInfo
il2cpp:0000000180662DA2                 test    byte ptr [rcx+12Fh], 2
il2cpp:0000000180662DA9                 jz      short loc_180662DB9
il2cpp:0000000180662DAB                 cmp     dword ptr [rcx+0E0h], 0
il2cpp:0000000180662DB2                 jnz     short loc_180662DB9
il2cpp:0000000180662DB4                 call    il2cpp_runtime_class_init
il2cpp:0000000180662DB9
il2cpp:0000000180662DB9 loc_180662DB9:                          ; CODE XREF: CommunityEntity$$AddUI+3F9↑j
il2cpp:0000000180662DB9                                         ; CommunityEntity$$AddUI+402↑j
il2cpp:0000000180662DB9                 xor     edx, edx        ; method
il2cpp:0000000180662DBB                 mov     rcx, r15        ; go
il2cpp:0000000180662DBE                 call    CommunityEntity$$RegisterUi
il2cpp:0000000180662DC3                 mov     rdx, cs:Method$UnityEngine_GameObject_GetComponent_RectTransform___ ; method
il2cpp:0000000180662DCA                 mov     rcx, r15        ; this
il2cpp:0000000180662DCD                 call    UnityEngine_GameObject$$GetComponent_object_
il2cpp:0000000180662DD2                 mov     rbx, rax
il2cpp:0000000180662DD5                 mov     rcx, cs:UnityEngine_Object_TypeInfo ; UnityEngine.Object_TypeInfo
il2cpp:0000000180662DDC                 test    byte ptr [rcx+12Fh], 2
il2cpp:0000000180662DE3                 jz      short loc_180662DF3
il2cpp:0000000180662DE5                 cmp     dword ptr [rcx+0E0h], 0
il2cpp:0000000180662DEC                 jnz     short loc_180662DF3
il2cpp:0000000180662DEE                 call    il2cpp_runtime_class_init
il2cpp:0000000180662DF3
il2cpp:0000000180662DF3 loc_180662DF3:                          ; CODE XREF: CommunityEntity$$AddUI+433↑j
il2cpp:0000000180662DF3                                         ; CommunityEntity$$AddUI+43C↑j
il2cpp:0000000180662DF3                 xor     edx, edx        ; method
il2cpp:0000000180662DF5                 mov     rcx, rbx        ; exists
il2cpp:0000000180662DF8                 call    UnityEngine_Object$$op_Implicit
il2cpp:0000000180662DFD                 xor     r14d, r14d
il2cpp:0000000180662E00                 test    al, al
il2cpp:0000000180662E02                 jz      loc_180662EA5
il2cpp:0000000180662E08                 mov     qword ptr [rbp+0E0h+var_C8.fields.x], r14
il2cpp:0000000180662E0C                 xor     r9d, r9d
il2cpp:0000000180662E0F                 movaps  xmm2, xmm6
il2cpp:0000000180662E12                 movaps  xmm1, xmm6
il2cpp:0000000180662E15                 lea     rcx, [rbp+0E0h+var_C8]
il2cpp:0000000180662E19                 call    sub_1811F0DD0
il2cpp:0000000180662E1E                 test    rbx, rbx
il2cpp:0000000180662E21                 jz      loc_180663155
il2cpp:0000000180662E27                 xor     r8d, r8d        ; method
il2cpp:0000000180662E2A                 mov     rdx, qword ptr [rbp+0E0h+var_C8.fields.x] ; value
il2cpp:0000000180662E2E                 mov     rcx, rbx        ; this
il2cpp:0000000180662E31                 call    UnityEngine_RectTransform$$set_anchorMin
il2cpp:0000000180662E36                 mov     qword ptr [rbp+0E0h+var_B0.fields.x], r14
il2cpp:0000000180662E3A                 xor     r9d, r9d
il2cpp:0000000180662E3D                 movaps  xmm2, xmm7
il2cpp:0000000180662E40                 movaps  xmm1, xmm7
il2cpp:0000000180662E43                 lea     rcx, [rbp+0E0h+var_B0]
il2cpp:0000000180662E47                 call    sub_1811F0DD0
il2cpp:0000000180662E4C                 xor     r8d, r8d        ; method
il2cpp:0000000180662E4F                 mov     rdx, qword ptr [rbp+0E0h+var_B0.fields.x] ; value
il2cpp:0000000180662E53                 mov     rcx, rbx        ; this
il2cpp:0000000180662E56                 call    UnityEngine_RectTransform$$set_anchorMax
il2cpp:0000000180662E5B                 mov     qword ptr [rbp+0E0h+var_A8.fields.x], r14
il2cpp:0000000180662E5F                 xor     r9d, r9d
il2cpp:0000000180662E62                 movaps  xmm2, xmm6
il2cpp:0000000180662E65                 movaps  xmm1, xmm6
il2cpp:0000000180662E68                 lea     rcx, [rbp+0E0h+var_A8]
il2cpp:0000000180662E6C                 call    sub_1811F0DD0
il2cpp:0000000180662E71                 xor     r8d, r8d        ; method
il2cpp:0000000180662E74                 mov     rdx, qword ptr [rbp+0E0h+var_A8.fields.x] ; value
il2cpp:0000000180662E78                 mov     rcx, rbx        ; this
il2cpp:0000000180662E7B                 call    UnityEngine_RectTransform$$set_offsetMin
il2cpp:0000000180662E80                 mov     qword ptr [rbp+0E0h+var_A0.fields.x], r14
il2cpp:0000000180662E84                 xor     r9d, r9d
il2cpp:0000000180662E87                 movaps  xmm2, xmm7
il2cpp:0000000180662E8A                 movaps  xmm1, xmm7
il2cpp:0000000180662E8D                 lea     rcx, [rbp+0E0h+var_A0]
il2cpp:0000000180662E91                 call    sub_1811F0DD0
il2cpp:0000000180662E96                 xor     r8d, r8d        ; method
il2cpp:0000000180662E99                 mov     rdx, qword ptr [rbp+0E0h+var_A0.fields.x] ; value
il2cpp:0000000180662E9D                 mov     rcx, rbx        ; this
il2cpp:0000000180662EA0                 call    UnityEngine_RectTransform$$set_offsetMax
il2cpp:0000000180662EA5
il2cpp:0000000180662EA5 loc_180662EA5:                          ; CODE XREF: CommunityEntity$$AddUI+452↑j
il2cpp:0000000180662EA5                 xor     r8d, r8d        ; method
il2cpp:0000000180662EA8                 mov     rdx, cs:StringLiteral_5982 ; key
il2cpp:0000000180662EAF                 mov     rcx, r13        ; this
il2cpp:0000000180662EB2                 call    JSON_Object$$GetArray
il2cpp:0000000180662EB7                 test    rax, rax
il2cpp:0000000180662EBA                 jz      loc_180663175
il2cpp:0000000180662EC0                 xor     edx, edx        ; method
il2cpp:0000000180662EC2                 mov     rcx, rax        ; this
il2cpp:0000000180662EC5                 call    JSON_Array$$GetEnumerator
il2cpp:0000000180662ECA                 mov     rbx, rax
il2cpp:0000000180662ECD                 mov     qword ptr [rbp+0E0h+var_C8.fields.x], rax
il2cpp:0000000180662ED1                 mov     rdi, [rbp+0E0h+arg_0]
il2cpp:0000000180662ED8                 nop     dword ptr [rax+rax+00000000h]
il2cpp:0000000180662EE0
il2cpp:0000000180662EE0 loc_180662EE0:                          ; CODE XREF: CommunityEntity$$AddUI+57A↓j
il2cpp:0000000180662EE0                 test    rbx, rbx
il2cpp:0000000180662EE3                 jz      loc_180663160
il2cpp:0000000180662EE9                 mov     r8, rbx
il2cpp:0000000180662EEC                 mov     rdx, cs:System_Collections_IEnumerator_TypeInfo ; System.Collections.IEnumerator_TypeInfo
il2cpp:0000000180662EF3                 xor     ecx, ecx
il2cpp:0000000180662EF5                 call    sub_1800F79C0
il2cpp:0000000180662EFA                 test    al, al
il2cpp:0000000180662EFC                 jz      short loc_180662F2C
il2cpp:0000000180662EFE                 mov     r8, rbx
il2cpp:0000000180662F01                 mov     rdx, cs:System_Collections_Generic_IEnumerator_Value__TypeInfo ; System.Collections.Generic.IEnumerator<Value>_TypeInfo
il2cpp:0000000180662F08                 xor     ecx, ecx
il2cpp:0000000180662F0A                 call    sub_1800F79C0
il2cpp:0000000180662F0F                 test    rax, rax
il2cpp:0000000180662F12                 jz      loc_18066315B
il2cpp:0000000180662F18                 xor     r9d, r9d        ; method
il2cpp:0000000180662F1B                 mov     r8, [rax+28h]   ; obj
il2cpp:0000000180662F1F                 mov     rdx, r15        ; go
il2cpp:0000000180662F22                 mov     rcx, rdi        ; this
il2cpp:0000000180662F25                 call    CommunityEntity$$CreateComponents
il2cpp:0000000180662F2A                 jmp     short loc_180662EE0
il2cpp:0000000180662F2C ; ---------------------------------------------------------------------------
il2cpp:0000000180662F2C
il2cpp:0000000180662F2C loc_180662F2C:                          ; CODE XREF: CommunityEntity$$AddUI+54C↑j
il2cpp:0000000180662F2C                 inc     r12d
il2cpp:0000000180662F2F                 mov     [rbp+0E0h+arg_18], r12d
il2cpp:0000000180662F36                 movsxd  rax, [rbp+0E0h+var_D8]
il2cpp:0000000180662F3A                 mov     rcx, [rbp+0E0h+var_D0]
il2cpp:0000000180662F3E                 mov     dword ptr [rcx+rax*4+4], 199h
il2cpp:0000000180662F46                 mov     rsi, [rbp+0E0h+var_E0]
il2cpp:0000000180662F4A                 jmp     short loc_180662F7D
il2cpp:0000000180662F4C ; ---------------------------------------------------------------------------
il2cpp:0000000180662F4C                 xor     r14d, r14d
il2cpp:0000000180662F4F                 mov     r13, [rbp+0E0h+var_90]
il2cpp:0000000180662F53                 mov     r15, [rbp+0E0h+var_88]
il2cpp:0000000180662F57                 mov     rbx, qword ptr [rbp+0E0h+var_C8.fields.x]
il2cpp:0000000180662F5B                 mov     rsi, [rbp+0E0h+var_E0]
il2cpp:0000000180662F5F                 mov     [rbp+0E0h+var_E0], rsi
il2cpp:0000000180662F63                 xorps   xmm6, xmm6
il2cpp:0000000180662F66                 movss   xmm7, cs:valueEnd
il2cpp:0000000180662F6E                 mov     r12d, [rbp+0E0h+arg_18]
il2cpp:0000000180662F75                 mov     rax, [rbp+0E0h+var_98]
il2cpp:0000000180662F79                 mov     [rbp+0E0h+var_D0], rax
il2cpp:0000000180662F7D
il2cpp:0000000180662F7D loc_180662F7D:                          ; CODE XREF: CommunityEntity$$AddUI+59A↑j
il2cpp:0000000180662F7D                 mov     edi, r12d
il2cpp:0000000180662F80                 test    rbx, rbx
il2cpp:0000000180662F83                 jz      short loc_180662F96
il2cpp:0000000180662F85                 mov     r8, rbx
il2cpp:0000000180662F88                 mov     rdx, cs:System_IDisposable_TypeInfo ; System.IDisposable_TypeInfo
il2cpp:0000000180662F8F                 xor     ecx, ecx
il2cpp:0000000180662F91                 call    sub_1800F79C0
il2cpp:0000000180662F96
il2cpp:0000000180662F96 loc_180662F96:                          ; CODE XREF: CommunityEntity$$AddUI+5D3↑j
il2cpp:0000000180662F96                 cmp     r12d, 0FFFFFFFFh
il2cpp:0000000180662F9A                 jz      short loc_180663014
il2cpp:0000000180662F9C                 movsxd  rax, r12d
il2cpp:0000000180662F9F                 mov     rcx, [rbp+0E0h+var_D0]
il2cpp:0000000180662FA3                 cmp     dword ptr [rcx+rax*4], 199h
il2cpp:0000000180662FAA                 jnz     short loc_180663014
il2cpp:0000000180662FAC                 dec     r12d
il2cpp:0000000180662FAF                 test    edi, edi
il2cpp:0000000180662FB1                 cmovs   r12d, edi
il2cpp:0000000180662FB5                 mov     [rbp+0E0h+arg_18], r12d
il2cpp:0000000180662FBC
il2cpp:0000000180662FBC loc_180662FBC:                          ; CODE XREF: CommunityEntity$$AddUI+66D↓j
il2cpp:0000000180662FBC                 xor     r8d, r8d        ; method
il2cpp:0000000180662FBF                 mov     rdx, cs:StringLiteral_16420 ; key
il2cpp:0000000180662FC6                 mov     rcx, r13        ; this
il2cpp:0000000180662FC9                 call    JSON_Object$$ContainsKey
il2cpp:0000000180662FCE                 test    al, al
il2cpp:0000000180662FD0                 mov     rdi, [rbp+0E0h+var_C0]
il2cpp:0000000180662FD4                 jz      loc_180662B10
il2cpp:0000000180662FDA                 mov     rdx, cs:Method$UnityEngine_GameObject_AddComponent_CommunityEntity_FadeOut___ ; method
il2cpp:0000000180662FE1                 mov     rcx, r15        ; this
il2cpp:0000000180662FE4                 call    UnityEngine_GameObject$$AddComponent_object_
il2cpp:0000000180662FE9                 mov     rbx, rax
il2cpp:0000000180662FEC                 xor     r9d, r9d        ; method
il2cpp:0000000180662FEF                 movaps  xmm2, xmm6      ; iDefault
il2cpp:0000000180662FF2                 mov     rdx, cs:StringLiteral_16420 ; key
il2cpp:0000000180662FF9                 mov     rcx, r13        ; this
il2cpp:0000000180662FFC                 call    JSON_Object$$GetFloat
il2cpp:0000000180663001                 test    rbx, rbx
il2cpp:0000000180663004                 jz      loc_180663170
il2cpp:000000018066300A                 movss   dword ptr [rbx+18h], xmm0
il2cpp:000000018066300F                 jmp     loc_180662B10
il2cpp:0000000180663014 ; ---------------------------------------------------------------------------
il2cpp:0000000180663014
il2cpp:0000000180663014 loc_180663014:                          ; CODE XREF: CommunityEntity$$AddUI+5EA↑j
il2cpp:0000000180663014                                         ; CommunityEntity$$AddUI+5FA↑j
il2cpp:0000000180663014                 test    rsi, rsi
il2cpp:0000000180663017                 jnz     loc_180663166
il2cpp:000000018066301D                 jmp     short loc_180662FBC
il2cpp:000000018066301F ; ---------------------------------------------------------------------------
il2cpp:000000018066301F
il2cpp:000000018066301F loc_18066301F:                          ; CODE XREF: CommunityEntity$$AddUI+2F1↑j
il2cpp:000000018066301F                 call    JSON_Object$$GetString
il2cpp:0000000180663024                 mov     rbx, rax
il2cpp:0000000180663027                 xor     r9d, r9d        ; method
il2cpp:000000018066302A                 mov     r8, cs:StringLiteral_16416 ; strDEFAULT
il2cpp:0000000180663031                 mov     rdx, cs:StringLiteral_3352 ; key
il2cpp:0000000180663038                 mov     rcx, r13        ; this
il2cpp:000000018066303B                 call    JSON_Object$$GetString
il2cpp:0000000180663040                 mov     [rsp+120h+method], r14 ; method
il2cpp:0000000180663045                 mov     r9, rax         ; str3
il2cpp:0000000180663048                 mov     r8, cs:StringLiteral_16419 ; str2
il2cpp:000000018066304F                 mov     rdx, rbx        ; str1
il2cpp:0000000180663052                 mov     rcx, cs:StringLiteral_16417 ; str0
il2cpp:0000000180663059                 call    System_String$$Concat_6470865648
il2cpp:000000018066305E                 mov     rbx, rax
il2cpp:0000000180663061                 mov     rcx, cs:UnityEngine_Debug_TypeInfo ; UnityEngine.Debug_TypeInfo
il2cpp:0000000180663068                 test    byte ptr [rcx+12Fh], 2
il2cpp:000000018066306F                 jz      short loc_18066307F
il2cpp:0000000180663071                 cmp     dword ptr [rcx+0E0h], 0
il2cpp:0000000180663078                 jnz     short loc_18066307F
il2cpp:000000018066307A                 call    il2cpp_runtime_class_init
il2cpp:000000018066307F
il2cpp:000000018066307F loc_18066307F:                          ; CODE XREF: CommunityEntity$$AddUI+6BF↑j
il2cpp:000000018066307F                                         ; CommunityEntity$$AddUI+6C8↑j
il2cpp:000000018066307F                 xor     edx, edx        ; method
il2cpp:0000000180663081                 mov     rcx, rbx        ; message
il2cpp:0000000180663084                 call    UnityEngine_Debug$$LogWarning
il2cpp:0000000180663089                 inc     r12d
il2cpp:000000018066308C                 mov     [rbp+0E0h+arg_18], r12d
il2cpp:0000000180663093                 movsxd  rax, [rbp+0E0h+var_D8]
il2cpp:0000000180663097                 mov     r13, [rbp+0E0h+var_D0]
il2cpp:000000018066309B                 mov     dword ptr [r13+rax*4+4], 1D9h
il2cpp:00000001806630A4                 mov     rsi, [rbp+0E0h+var_E0]
il2cpp:00000001806630A8                 jmp     short loc_1806630D6
il2cpp:00000001806630AA ; ---------------------------------------------------------------------------
il2cpp:00000001806630AA
il2cpp:00000001806630AA loc_1806630AA:                          ; CODE XREF: CommunityEntity$$AddUI+189↑j
il2cpp:00000001806630AA                 inc     r12d
il2cpp:00000001806630AD                 mov     [rbp+0E0h+arg_18], r12d
il2cpp:00000001806630B4                 mov     r13, [rbp+0E0h+var_D0]
il2cpp:00000001806630B8                 mov     dword ptr [r13+rbx*4+4], 1D9h
il2cpp:00000001806630C1                 jmp     short loc_1806630D6
il2cpp:00000001806630C3 ; ---------------------------------------------------------------------------
il2cpp:00000001806630C3                 mov     rsi, [rbp+0E0h+var_E0]
il2cpp:00000001806630C7                 mov     r12d, [rbp+0E0h+arg_18]
il2cpp:00000001806630CE                 mov     r13, [rbp+0E0h+var_98]
il2cpp:00000001806630D2                 mov     rdi, [rbp+0E0h+var_C0]
il2cpp:00000001806630D6
il2cpp:00000001806630D6 loc_1806630D6:                          ; CODE XREF: CommunityEntity$$AddUI+6F8↑j
il2cpp:00000001806630D6                                         ; CommunityEntity$$AddUI+711↑j
il2cpp:00000001806630D6                 test    rdi, rdi
il2cpp:00000001806630D9                 jz      short loc_1806630EC
il2cpp:00000001806630DB                 mov     r8, rdi
il2cpp:00000001806630DE                 mov     rdx, cs:System_IDisposable_TypeInfo ; System.IDisposable_TypeInfo
il2cpp:00000001806630E5                 xor     ecx, ecx
il2cpp:00000001806630E7                 call    sub_1800F79C0
il2cpp:00000001806630EC
il2cpp:00000001806630EC loc_1806630EC:                          ; CODE XREF: CommunityEntity$$AddUI+729↑j
il2cpp:00000001806630EC                 cmp     r12d, 0FFFFFFFFh
il2cpp:00000001806630F0                 jz      short loc_180663100
il2cpp:00000001806630F2                 movsxd  rax, r12d
il2cpp:00000001806630F5                 cmp     dword ptr [r13+rax*4+0], 1D9h
il2cpp:00000001806630FE                 jz      short loc_180663109
il2cpp:0000000180663100
il2cpp:0000000180663100 loc_180663100:                          ; CODE XREF: CommunityEntity$$AddUI+740↑j
il2cpp:0000000180663100                 test    rsi, rsi
il2cpp:0000000180663103                 jnz     loc_1806631A3
il2cpp:0000000180663109
il2cpp:0000000180663109 loc_180663109:                          ; CODE XREF: CommunityEntity$$AddUI+DB↑j
il2cpp:0000000180663109                                         ; CommunityEntity$$AddUI+11A↑j ...
il2cpp:0000000180663109                 mov     rbx, [rbp+0E0h+arg_8]
il2cpp:0000000180663110                 movaps  xmm6, [rbp+0E0h+var_40]
il2cpp:0000000180663117                 movaps  xmm7, [rbp+0E0h+var_50]
il2cpp:000000018066311E                 lea     rsp, [rbp+0B0h]
il2cpp:0000000180663125                 pop     r15
il2cpp:0000000180663127                 pop     r14
il2cpp:0000000180663129                 pop     r13
il2cpp:000000018066312B                 pop     r12
il2cpp:000000018066312D                 pop     rdi
il2cpp:000000018066312E                 pop     rsi
il2cpp:000000018066312F                 pop     rbp
il2cpp:0000000180663130                 retn
il2cpp:0000000180663131 ; ---------------------------------------------------------------------------
il2cpp:0000000180663131
il2cpp:0000000180663131 loc_180663131:                          ; CODE XREF: CommunityEntity$$AddUI+297↑j
il2cpp:0000000180663131                 call    null_ref_exception
il2cpp:0000000180663131 ; ---------------------------------------------------------------------------
il2cpp:0000000180663136                 db 90h
il2cpp:0000000180663137 ; ---------------------------------------------------------------------------
il2cpp:0000000180663137
il2cpp:0000000180663137 loc_180663137:                          ; CODE XREF: CommunityEntity$$AddUI+365↑j
il2cpp:0000000180663137                 call    sub_1803B97C0
il2cpp:000000018066313C                 xor     edx, edx
il2cpp:000000018066313E                 mov     rcx, rax
il2cpp:0000000180663141                 call    sub_1803B99B0
il2cpp:0000000180663146 ; ---------------------------------------------------------------------------
il2cpp:0000000180663146
il2cpp:0000000180663146 loc_180663146:                          ; CODE XREF: CommunityEntity$$AddUI+36F↑j
il2cpp:0000000180663146                 call    sub_1803B97F0
il2cpp:000000018066314B                 xor     edx, edx
il2cpp:000000018066314D                 mov     rcx, rax
il2cpp:0000000180663150                 call    sub_1803B99B0
il2cpp:0000000180663155 ; ---------------------------------------------------------------------------
il2cpp:0000000180663155
il2cpp:0000000180663155 loc_180663155:                          ; CODE XREF: CommunityEntity$$AddUI+471↑j
il2cpp:0000000180663155                 call    null_ref_exception
il2cpp:0000000180663155 ; ---------------------------------------------------------------------------
il2cpp:000000018066315A                 db 90h
il2cpp:000000018066315B ; ---------------------------------------------------------------------------
il2cpp:000000018066315B
il2cpp:000000018066315B loc_18066315B:                          ; CODE XREF: CommunityEntity$$AddUI+562↑j
il2cpp:000000018066315B                 call    null_ref_exception
il2cpp:0000000180663160 ; ---------------------------------------------------------------------------
il2cpp:0000000180663160
il2cpp:0000000180663160 loc_180663160:                          ; CODE XREF: CommunityEntity$$AddUI+533↑j
il2cpp:0000000180663160                 call    null_ref_exception
il2cpp:0000000180663160 ; ---------------------------------------------------------------------------
il2cpp:0000000180663165                 align 2
il2cpp:0000000180663166
il2cpp:0000000180663166 loc_180663166:                          ; CODE XREF: CommunityEntity$$AddUI+667↑j
il2cpp:0000000180663166                 xor     edx, edx
il2cpp:0000000180663168                 mov     rcx, rsi
il2cpp:000000018066316B                 call    sub_1803B99B0
il2cpp:0000000180663170 ; ---------------------------------------------------------------------------
il2cpp:0000000180663170
il2cpp:0000000180663170 loc_180663170:                          ; CODE XREF: CommunityEntity$$AddUI+654↑j
il2cpp:0000000180663170                 call    null_ref_exception
il2cpp:0000000180663175 ; ---------------------------------------------------------------------------
il2cpp:0000000180663175
il2cpp:0000000180663175 loc_180663175:                          ; CODE XREF: CommunityEntity$$AddUI+50A↑j
il2cpp:0000000180663175                 call    null_ref_exception
il2cpp:000000018066317A ; ---------------------------------------------------------------------------
il2cpp:000000018066317A
il2cpp:000000018066317A loc_18066317A:                          ; CODE XREF: CommunityEntity$$AddUI+3D4↑j
il2cpp:000000018066317A                 call    null_ref_exception
il2cpp:000000018066317F ; ---------------------------------------------------------------------------
il2cpp:000000018066317F
il2cpp:000000018066317F loc_18066317F:                          ; CODE XREF: CommunityEntity$$AddUI+3C1↑j
il2cpp:000000018066317F                 call    null_ref_exception
il2cpp:0000000180663184 ; ---------------------------------------------------------------------------
il2cpp:0000000180663184
il2cpp:0000000180663184 loc_180663184:                          ; CODE XREF: CommunityEntity$$AddUI+3AB↑j
il2cpp:0000000180663184                 call    null_ref_exception
il2cpp:0000000180663189 ; ---------------------------------------------------------------------------
il2cpp:0000000180663189
il2cpp:0000000180663189 loc_180663189:                          ; CODE XREF: CommunityEntity$$AddUI+348↑j
il2cpp:0000000180663189                 call    null_ref_exception
il2cpp:000000018066318E ; ---------------------------------------------------------------------------
il2cpp:000000018066318E
il2cpp:000000018066318E loc_18066318E:                          ; CODE XREF: CommunityEntity$$AddUI+22B↑j
il2cpp:000000018066318E                 call    null_ref_exception
il2cpp:0000000180663193 ; ---------------------------------------------------------------------------
il2cpp:0000000180663193
il2cpp:0000000180663193 loc_180663193:                          ; CODE XREF: CommunityEntity$$AddUI+1B4↑j
il2cpp:0000000180663193                 call    null_ref_exception
il2cpp:0000000180663198 ; ---------------------------------------------------------------------------
il2cpp:0000000180663198
il2cpp:0000000180663198 loc_180663198:                          ; CODE XREF: CommunityEntity$$AddUI+1A3↑j
il2cpp:0000000180663198                 call    null_ref_exception
il2cpp:000000018066319D ; ---------------------------------------------------------------------------
il2cpp:000000018066319D
il2cpp:000000018066319D loc_18066319D:                          ; CODE XREF: CommunityEntity$$AddUI+170↑j
il2cpp:000000018066319D                 call    null_ref_exception
il2cpp:000000018066319D ; ---------------------------------------------------------------------------
il2cpp:00000001806631A2                 db 90h
il2cpp:00000001806631A3 ; ---------------------------------------------------------------------------
il2cpp:00000001806631A3
il2cpp:00000001806631A3 loc_1806631A3:                          ; CODE XREF: CommunityEntity$$AddUI+753↑j
il2cpp:00000001806631A3                 xor     edx, edx
il2cpp:00000001806631A5                 mov     rcx, rsi
il2cpp:00000001806631A8                 call    sub_1803B99B0
il2cpp:00000001806631A8 ; ---------------------------------------------------------------------------
il2cpp:00000001806631AD                 align 2
il2cpp:00000001806631AE
il2cpp:00000001806631AE loc_1806631AE:                          ; CODE XREF: CommunityEntity$$AddUI+F8↑j
il2cpp:00000001806631AE                 call    null_ref_exception
il2cpp:00000001806631AE ; ---------------------------------------------------------------------------
il2cpp:00000001806631B3                 db 0CCh
il2cpp:00000001806631B3 ; } // starts at 1806629B0
il2cpp:00000001806631B4 algn_1806631B4:                         ; DATA XREF: .pdata:00000001836A9098↓o
il2cpp:00000001806631B4                 align 20h
il2cpp:00000001806631B4 CommunityEntity$$AddUI endp
il2cpp:00000001806631B4
```