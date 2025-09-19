```mermaid
---
config:
  look: neo
  layout: elk
  theme: neutral
---
classDiagram
    direction BT

    class `cpsw_entry_adapt.h : IEntryAdapt`:::cpsw_entry_adapt_h {
        #ie_ : boost::shared_ptr&lt;CEntryImpl const> 
        #p_ : ConstPath
        #uniq_ : CEntryImpl::UniqueHandle 

        +IEntryAdapt(CShObj::Key &, ConstPath, boost::shared_ptr&lt;CEntryImpl const>)
        #postCreateHook() void
        -setUnique(CEntryImpl::UniqueHandle) void 

        +getName() char&ast;
        +getDescription() char&ast;
        +getPollSecs() double
        +getSize() uint64_t
        +isHub() Hub
        +getPath() Path
        +getEncoding() Encoding
        +getConstPath() ConstPath
        +getNelms() unsigned int
        +dump(FILE *) void
        +dump() void
        +open() void
        +close() void
        +~IEntryAdapt()
        +check_interface(ConstPath p)$ template &lt;typename INTERF> static INTERF
        +check_interface(ConstPath p)$ template &lt;typename ADAPT, typename IMPL> static ADAPT
    }

    class `cpsw_sval.h : IIntEntryAdapt`:::cpsw_sval_h {
        +nelms_ : int
        -IIntEntryAdapt(CShObj::Key &, ConstPath, boost::shared_ptr&lt;CIntEntryImpl const>)
        -isSigned(void) bool
        -getLsBit(void) int
        -getSizeBits(void) uint64_t
        -getWordSwap(void) unsigned int 
        -getEncoding(void) Encoding
        -getMode(void) Mode
        -getEnum(void) Enum
        -nelmsFromIdx(IndexRange *) unsigned int
        -~IIntEntryAdapt(void)
        #asIntEntry(void) boost::shared_ptr&lt;CIntEntryImpl>
        #getVal(uint8_t *, unsigned int, unsigned int, SlicedPathIterator *) unsigned int
        #getVal(AsyncIO, uint8_t *, unsigned int, unsigned int, SlicedPathIterator *) unsigned int
        #checkNelms(unsigned int, SlicedPathIterator *) unsigned int
        #setVal(uint8_t *, unsigned int, unsigned int, IndexRange *) unsigned int
        #setVal<float>(float *, unsigned int, IndexRange *) unsigned int
        #setVal<double>(double *, unsigned int, IndexRange *) unsigned int
        #getVal<double>(double *, unsigned int, SlicedPathIterator *) unsigned int
        #getVal<double>(AsyncIO, double *, unsigned int, SlicedPathIterator *) unsigned int
        #getVal<float>(float *, unsigned int, SlicedPathIterator *) unsigned int
        #getVal<float>(AsyncIO, float *, unsigned int, SlicedPathIterator *) unsigned int
        #setVal<unsigned char>(unsigned char *, unsigned int, IndexRange *) unsigned int
        #setVal<unsigned short>(unsigned short *, unsigned int, IndexRange *) unsigned int
        #setVal<unsigned int>(unsigned int *, unsigned int, IndexRange *) unsigned int
        #setVal<unsigned long>(unsigned long *, unsigned int, IndexRange *) unsigned int
        #getVal<unsigned char>(AsyncIO, unsigned char *, unsigned int, IndexRange *) unsigned int
        #getVal<unsigned char>(unsigned char *, unsigned int, IndexRange *) unsigned int
        #getVal<unsigned short>(AsyncIO, unsigned short *, unsigned int, IndexRange *) unsigned int
        #getVal<unsigned short>(unsigned short *, unsigned int, IndexRange *) unsigned int
        #getVal<unsigned int>(AsyncIO, unsigned int *, unsigned int, IndexRange *) unsigned int
        #getVal<unsigned int>(unsigned int *, unsigned int, IndexRange *) unsigned int
        #getVal<unsigned long>(AsyncIO, unsigned long *, unsigned int, IndexRange *) unsigned int
        #getVal<unsigned long>(unsigned long *, unsigned int, IndexRange *) unsigned int
    }
    
    class `cpsw_api_user.h : IScalVal_Base`:::cpsw_api_user_h {
        <<interface>>
        +getSizeBits(void)* uint64_t
        +isSigned(void)* bool
        +getEnum(void)* Enum
        +~IScalVal_Base(void)
        +create(ConstPath) ScalVal_Base$
    }
    class `cpsw_api_user.h : IVal_Base`:::cpsw_api_user_h {
        <<interface>>
        +Mode : enum Mode
        +Encoding: enum
        +getNelms()* unsigned int
        +getPath()* Path
        +getConstPath()* ConstPath
        +getEncoding()* Encoding;
        +~IVal_Base()
        +create(ConstPath)$ Val_Base  
    }
    class Mode {
        <<enumeration>>
        RO = 1
        WO = 2
        RW = 3
    }
    class Encoding {
        <<enumeration>>
        NONE = 0
        ASCII
        EBCDIC
        UTF_8
        UTF_16
        UTF_32
        ISO_8859_1 = 16
        ISO_8859_2
        ISO_8859_3
        ISO_8859_4
        ISO_8859_5
        ISO_8859_6
        ISO_8859_7
        ISO_8859_8
        ISO_8859_9
        ISO_8859_10
        ISO_8859_11
        ISO_8859_13 = ISO_8859_11 + 2
        ISO_8859_14
        ISO_8859_15
        ISO_8859_16
        IEEE_754 = 0x0f00
        CUSTOM   = 0x1000
    }
    class `cpsw_api_user.h : IEntry`:::cpsw_api_user_h {
        <<interface>>
        +getName()* char&ast;
        +getSize()* uint64_t
        +getDescription()* char&ast;
        +getPollSecs()* double
        +isHub()* Hub
        +dump(FILE *)* void
        +dump()* void
        +~IEntry(void)
    }
    class `cpsw_api_user.h : IScalVal_RO`:::cpsw_api_user_h {
        <<interface>>
        +getVal(uint64_t *, unsigned int, IndexRange *)* unsigned int
        +getVal(AsyncIO, uint64_t *, unsigned int, IndexRange *)* unsigned int
        +getVal(uint32_t *, unsigned int, IndexRange *)* unsigned int
        +getVal(AsyncIO, uint32_t *, unsigned int, IndexRange *)* unsigned int
        +getVal(uint16_t *, unsigned int, IndexRange *)* unsigned int
        +getVal(AsyncIO, uint16_t *, unsigned int, IndexRange *)* unsigned int
        +getVal(uint8_t *, unsigned int, IndexRange *)* unsigned int
        +getVal(AsyncIO, uint8_t *, unsigned int, IndexRange *)* unsigned int
        +getVal(CString *, unsigned int, IndexRange *)* unsigned int
        +getVal(AsyncIO, CString *, unsigned int, IndexRange *)* unsigned int
        +~IScalVal_RO(void)
        +create(ConstPath)$ ScalVal_RO
    }
    class `cpsw_api_user.h : IScalVal`:::cpsw_api_user_h {
        +create(ConstPath) ScalVal$
        +~IScalVal(void)
    }
    class `cpsw_sval.h : CScalVal_ROAdapt`:::cpsw_sval_h {
        +CScalVal_ROAdapt(CShObj::Key &, ConstPath, boost::shared_ptr&lt;CIntEntryImpl const>)
        +getVal(uint64_t *, unsigned int, IndexRange *) unsigned int
        +getVal(AsyncIO, uint64_t *, unsigned int, IndexRange *) unsigned int
        +getVal(uint32_t *, unsigned int, IndexRange *) unsigned int
        +getVal(AsyncIO, uint32_t *, unsigned int, IndexRange *) unsigned int
        +getVal(uint16_t *, unsigned int, IndexRange *) unsigned int
        +getVal(AsyncIO, uint16_t *, unsigned int, IndexRange *) unsigned int
        +getVal(uint8_t *, unsigned int, IndexRange *) unsigned int
        +getVal(AsyncIO, uint8_t *, unsigned int, IndexRange *) unsigned int
        +getVal(CString *, unsigned int, IndexRange *) unsigned int
        +getVal(AsyncIO, CString *, unsigned int, IndexRange *) unsigned int
    }
    class `cpsw_sval.h : CScalVal_Adapt`:::cpsw_sval_h {
        +CScalValAdapt(CShObj::Key &, ConstPath, boost::shared_ptr&lt;CIntEntryImpl const>)
    }
    class `cpsw_shared_obj.h : Key`:::cpsw_shared_obj_h {
        -good_ : bool
        -friend class CShObj
        -Key()
        -Key &operator=(const Key &orig)
        +Key(Key &orig)
        +isValid() : bool
    }
    class `cpsw_shared_obj.h : CShObj`:::cpsw_shared_obj_h {
        +class StolenKeyError
        +class MustNotAssignError
        +class MustNotCopyError
        +class CloneNotImplementedError
        -self_ : boost::weak_ptr&lt;CShObj>
        #ConstShObj : typedef boost::shared_ptr&lt;CShObj const>
        #ShObj : typedef shared_ptr&lt;CCShObj>
        #ConstShObj : typedef shared_ptr&lt;const CShObj>
        #class Key

        +sh_ocnt_()$ CpswObjCounter&
        #start() void
        #postHook(orig : ConstShObj) void
        -operator=(const CShObj &) CShObj&
        -CShObj(const CShObj &)
        -CShObj(CShObj &)
        #CShObj(const CShObj &, CShObj::Key &)
        +CShObj(CShObj::Key &)
        -startRedirector() void
        #getSelfAs() template &lt;typename AS> AS
        #getSelfAsConst() template &lt;typename AS> AS
        +~CShObj()
        +clone(CShObj::Key &) CShObj&ast;
        +clone(T in)$ template &lt;typename T> T
        +cloneUnchecked(T in)$ template &lt;typename T> T
        +create()$ template &lt;typename T> T
        +create(A1 a1)$ template &lt;typename T, typename A1> T
        +create(A1 a1, A2 a2)$ template &lt;typename T, typename A1, typename A2> T
        +create(A1 a1, A2 a2, A3 a3)$ template &lt;typename T, typename A1, typename A2, typename A3> T
        +create(A1 a1, A2 a2, A3 a3, A4 a4)$ template &lt;typename T, typename A1, typename A2, typename A3, typename A4> T
        +create(A1 a1, A2 a2, A3 a3, A4 a4, A5 a5)$ template &lt;typename T, typename A1, typename A2, typename A3, typename A4, typename A5> T
        +create(A1 a1, A2 a2, A3 a3, A4 a4, A5 a5, A6 a6)$ template &lt;typename T, typename A1, typename A2, typename A3, typename A4, typename A5, typename A6> T
        +create(A1 a1, A2 a2, A3 a3, A4 a4, A5 a5, A6 a6, A7 a7)$ template &lt;typename T, typename A1, typename A2, typename A3, typename A4, typename A5, typename A6, typename A7> T
        -setSelf(P* p)$ boost::shared_ptr&lt;P>
        -postConstruct(P* p, ConstShObj orig=ConstShObj())$ boost::shared_ptr&lt;P>
    }
    `cpsw_entry_adapt.h : IEntryAdapt` ..|> `cpsw_api_user.h : IVal_Base`: implements
    `cpsw_entry_adapt.h : IEntryAdapt` --|> `cpsw_shared_obj.h : CShObj`
    `cpsw_sval.h : IIntEntryAdapt` --|> `cpsw_entry_adapt.h : IEntryAdapt`
    `cpsw_sval.h : IIntEntryAdapt` ..|> `cpsw_api_user.h : IScalVal_Base`: implements
    `cpsw_api_user.h : IScalVal_RO` --|> `cpsw_api_user.h : IScalVal_Base`
    `cpsw_api_user.h : IScalVal_Base` --|> `cpsw_api_user.h : IVal_Base`
    `cpsw_api_user.h : IVal_Base` --|> `cpsw_api_user.h : IEntry`
    `cpsw_api_user.h : IVal_Base` *-- Mode
    `cpsw_api_user.h : IVal_Base` *-- Encoding
    `cpsw_api_user.h : IScalVal` --|> `cpsw_api_user.h : IScalVal_RO`
    `cpsw_sval.h : CScalVal_ROAdapt` ..|> `cpsw_api_user.h : IScalVal_RO`: implements
    `cpsw_sval.h : CScalVal_Adapt` ..|> `cpsw_api_user.h : IScalVal`: implements
    `cpsw_sval.h : CScalVal_ROAdapt` --|> `cpsw_sval.h : IIntEntryAdapt`
    `cpsw_sval.h : CScalVal_Adapt` --|> `cpsw_sval.h : CScalVal_ROAdapt`
    `cpsw_shared_obj.h : CShObj` *-- `cpsw_shared_obj.h : Key` : protected nested class
    `cpsw_shared_obj.h : Key` *-- `cpsw_shared_obj.h : CShObj` : friend class

%% Color schemes
classDef cpsw_api_user_h fill:#CCFFE5
classDef cpsw_sval_h fill:#CCFFFF
classDef cpsw_entry_adapt_h fill:#FFFFCC
classDef cpsw_shared_obj_h fill:#E5CCFF
```
