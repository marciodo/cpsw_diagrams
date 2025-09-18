```mermaid
---
config:
    layout: elk
---

classDiagram
    direction BT

namespace YAML {
    class Node {

    }
}

namespace cpsw_sval.h {
    class CIntEntryImpl {
        +dumpYamlPart(&n YAML::Node)* void
        +dumpMyConfigToYaml(p Path, &n:YAML::Node)* uint64_t
        +loadMyConfigFromYaml(p:Path, &n:YAML::Node)* uint64_t
        +getMode()* Mode
    }

    class CScalVal_ROAdapt {
        +CScalVal_ROAdapt(CShObj::Key &, ConstPath, boost::shared_ptr<CIntEntryImpl const>)
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

    class ScalVal_ROAdapt {
        <<typedef>>
    }
    class CScalVal_WOAdapt
    class ScalVal_WOAdapt {
        <<typedef>>
    }
    class CScalVal_Adapt {
        +CScalValAdapt(CShObj::Key &, ConstPath, boost::shared_ptr<CIntEntryImpl const>)
    }

    class ScalVal_Adapt {
        <<typedef>>
    }

    class IIntEntryAdapt {
        %%private:
        +nelms_ : int

        %%public:
        -IIntEntryAdapt(CShObj::Key &, ConstPath, boost::shared_ptr<CIntEntryImpl const>)
        -isSigned(void) bool
        -getLsBit(void) int
        -getSizeBits(void) uint64_t
        -getWordSwap(void) unsigned int 
        -getEncoding(void) Encoding
        -getMode(void) Mode
        -getEnum(void) Enum
        -nelmsFromIdx(IndexRange *) unsigned int
        -~IIntEntryAdapt(void)

        %%protected:
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
}
ScalVal_ROAdapt ..> CScalVal_ROAdapt:shared_ptr
ScalVal_WOAdapt ..> CScalVal_WOAdapt:shared_ptr
ScalVal_Adapt ..> CScalVal_Adapt:shared_ptr
CScalVal_ROAdapt ..|> IScalVal_RO: implements
CScalVal_ROAdapt --|> IIntEntryAdapt: virtual
CScalVal_Adapt --|> CScalVal_ROAdapt: virtual
CScalVal_Adapt --|> CScalVal_WOAdapt: virtual
CScalVal_Adapt ..|> IScalVal: implements
IIntEntryAdapt --|> IEntryAdapt
IIntEntryAdapt ..|> IScalVal_Base: implements
CIntEntryImpl ..|> CEntryImpl

namespace cpsw_yaml.h {
    class CYamlSupportBase {
        <<interface>>
        +dumpYamlPart(&node YAML::Node)* void
    }
}
CYamlSupportBase --|> IYamlSupportBase: virtual

namespace cpsw_shared_obj.h {
    class Key {
        -good_ : bool
        -friend class CShObj
        -Key()
        -Key &operator=(const Key &orig)
        +Key(Key &orig)
        +isValid() : bool
    }

    class CShObj {
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
}

CShObj *-- Key
Key *-- CShObj

namespace cpsw_mmio_dev.h {
    class CMMIOAddressImpl {
        +read(node:CompositePathIterator*, args:CReadArgs*)* uint64_t
        +write(node:CompositePathIterator*, args:CWriteArgs*)* uint64_t
    }
}
CMMIOAddressImpl --|> CAddressImpl

namespace cpsw_address.h {
    class IAddress {
        <<interface>>
        +getEntryImpl()* EntryImpl
    }

    class Address {
        <<typedef>>
    }

    class CAddressImpl {
        -child_ : EntryImpl
        -nemls_ : unsigned
        -openCount_ : cpsw::atomic<int>
        -isSync_ : bool
        #byteOrder_ : ByteOrder
        +read(node:CompositePathIterator*, args:CReadArgs*)* uint64_t
        +read(args:CReadArgs*)* uint64_t
        +write(node:CompositePathIterator*, args:CWriteArgs*)* uint64_t
        +write(args:CWriteArgs*)* uint64_t
        +dispatchRead(node:CompositePathIterator*, args:CReadArgs*)* uint64_t
        +dispatchWrite(node:CompositePathIterator*, args:CWriteArgs*)* uint64_t
    }

    class AddressImpl {
        <<typedef>>
    }

    class CReadArgs {
        +cacheable_ : IField::Cacheable
        +dst_ : uint8_t*
        +nbytes_ : unsigned
        +off_ : uint64_t
        +timeout_ : CTimeout
        +aio_ : AsyncIO
        +CReadArgs()
    }
}
Address ..> IAddress:shared_ptr
AddressImpl ..> CAddressImpl:shared_ptr
IAddress --|> IChild
CAddressImpl --|> IAddress

namespace cpsw_path.h_or.cc {
    class IPathImpl {
        +append(Address, int f, int t)* void
        +tailAsPathEntry()* PathEntry
        +originAsDevImpl()* ConstDevImpl
        +parentAsDevImpl()* ConstDevImpl                                                                   
        +toPathImpl(Path p)$ IPathImpl*
    }

    class CPathImpl {
        -originDev_ : ConstDevImpl
        +CPathImpl()
        +CPathImpl(Hub)
        +CPathImpl(DevImpl)
        +CPathImpl(CPathImpl&)
        +clear()void
        +clear(Hub) void
        +clear(ConstDevImpl) void
        +dump(FILE *f) void
        +toString() std::string
        +size() int
        +empty() bool
        +begin() CPathImpl::iterator
        +begin() CPathImpl::const_iterator
        +findByName(char *name) Path
        +clone() Path
        +cloneAsPathImpl() PathImpl
        +intersect(ConstPath p) Path
        +isIntersecting(ConstPath p) bool
        +hasParent(CPathImpl::const_reverse_iterator &i)$ bool
        +hasParent(CPathImpl::reverse_iterator &i)$ bool
        +up() Child
        +tailAsPathEntry() PathEntry
        +tail() Child
        +getNelms() unsigned
        +verifyAtTail(ConstPath p) bool
        +verifyAtTail(ConstDevImpl c) bool
        +append(ConstPath p) void
        +append(Address) void
        +append(Address, int, int) void
        +concat(ConstPath p) Path
        +parentAsDevImpl() ConstDevImpl
        +parent() Hub
        +origin() Hub
        +originAsDevImpl() ConstDevImpl
        +getTailFrom() unsigned
        +getTailTo() unsigned
        +explore(IPathVisitor *) void
        +processYamlConfig(YAML::Node &, bool) uint64_t
        +dumpConfigToYaml(&node : YAML::Node) uint64_t
        +loadConfigFromYaml(YAML::Node &node) uint64_t
        +loadConfigFromYamlFile(filename : const char*, incdir : char*=0) uint64_t
        ~CPathImpl()
        #explore_r(struct explorer_ctxt*) void
        #explore_children_r(ConstDevImpl, struct explorer_ctxt*) void
    }

    class ConstDevImpl {
        <<typedef>>
    }

    class CDevImpl {

    }

    class PathImpl {
        <<typedef>>
    }

    class PathEntry {
        <<struct>>
        c_p_ : Address
        address_pvt_ : shared_ptr<void>
        idxf_ : int
        idxt_ : int
        nelmsLeft_ : unsigned
        PathEntry(a:Address, idxf:int=0, idxt:int=-1, nelmsLeft:unsigned=1)
    }

    class PathEntryContainer {
        <<typedef>>
    }
}
IPathImpl --|> IPath
CPathImpl ..|> IPathImpl: implements
CPathImpl --|> PathEntryContainer
PathImpl ..> CPathImpl: shared_ptr
ConstDevImpl ..> CDevImpl: shared_ptr
PathEntryContainer ..> PathEntry: std#colon;#colon;vector

namespace cpsw_api_user.h {
    class IVal_Base {
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

    class Val_Base {
        <<typedef>>
    }

    class IYamlSupportBase {
        <<interface>>
        +dumpYaml(&n:YAML::Node)* void
        +MIN_SUPPORTED_SCHEMA=3 : const int$
        +MAX_SUPPORTED_SCHEMA=3 : const int$
    }

    class IPath {
        +findByName(const char *name)* Path
        +up()* Child
        +empty()* bool
        +size()* int
        +clear()* void
        +clear(Hub)* void
        +origin()* Hub
        +parent()* Hub
        +tail()* Child
        +toString()* std::string
        +dump(FILE *)* void
        +verifyAtTail(ConstPath p)* bool
        +append(ConstPath p)* bool
        +concat(ConstPath p)* bool
        +clone()* Path
        +intersect(ConstPath p)* Path
        +isIntersecting(ConstPath p)* bool
        +getNelms()* unsigned
        +getTailFrom()* unsigned
        +getTailTo()* unsigned
        +explore(IPathVisitor *)* void
        +dumpConfigToYaml (&tmplt : YAML::Node)* uint64_t
        +loadConfigFromYaml(&config : YAML::Node)* uint64_t
        +loadConfigFromYamlFile(filename : const char*, *incdir : const char=0)* uint64_t
        +create()$ Path
        +create(const char*)$ Path
        +create(Hub)$ Path
        +loadYamlFile(char *fileName, char *rootName = "root", char *yamlDir = 0, IYamlFixup *fixup = 0)$ Path
        +loadYamlStream(std::istream &yaml, char *rootName = 0, char *yamlDir = 0, IYamlFixup *fixup = 0)$ Path
        +loadYamlStream(char *yaml, char *rootName = "root", char *yamlDir = 0, IYamlFixup *fixup = 0)$ Path
    }

    class Path {
        <<typedef>>
    }

    class IEntry {
        <<interface>>
        +getName()* char*
        +getSize()* uint64_t
        +getDescription()* char*
        +getPollSecs()* double
        +isHub()* Hub
        +dump(FILE *)* void
        +dump()* void
        +~IEntry(void);
    }

    class IHub
    class IChild

    class Child {
        <<typedef>>
    }

    class IScalVal {
        +create(ConstPath) ScalVal$
        +~IScalVal(void)
    }
    
    class IScalVal_Base {
        <<interface>>
        +getSizeBits(void)* uint64_t
        +isSigned(void)* bool
        +getEnum(void)* Enum
        +~IScalVal_Base(void)
        +create(ConstPath) ScalVal_Base$
    }

    class IScalVal_RO {
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

    class IScalVal_WO {
        <<interface>>
    }

    class ScalVal {
        <<typedef>>
    }

    class ScalVal_Base {
        <<typedef>>
    }
    
    class ScalVal_RO {
        <<typedef>>
    }
    
    class ScalVal_WO {
        <<typedef>>
    }
    
}
Path ..> IPath: shared_ptr
Child ..> IChild: shared_ptr
ScalVal ..> IScalVal: shared_ptr
ScalVal_Base ..> IScalVal_Base: shared_ptr
ScalVal_RO ..> IScalVal_RO: shared_ptr
ScalVal_WO ..> IScalVal_WO: shared_ptr
Val_Base ..> IVal_Base: shared_ptr
IScalVal --|> IScalVal_RO: virtual
IScalVal --|> IScalVal_WO: virtual
IScalVal_RO --|> IScalVal_Base: virtual
IScalVal_Base --|> IVal_Base: virtual
IVal_Base --|> IEntry: virtual
IHub --|> IEntry: virtual
IVal_Base *-- Mode
IVal_Base *-- Encoding
IChild --|> IEntry

namespace cpsw_api_builder.h {
    class IField {

    }

    class Field {
        <<typedef>>
    }
    
    class Cacheable {
        <<enumeration>>
        UNKNOWN_CACHEABLE = 0
        NOT_CACHEABLE
        WT_CACHEABLE
        WB_CACHEABLE
    }
    
    class IIntField {

    }

    class IntField {
        <<typedef>>
    }

    class IYamlSupport {
        <<namespace>>
        dumpYamlFile(top:Entry, filename:const char*,root_entry_name:const char*) void
        buildHierarchy(yaml_name:const char*, root_name:const char* = "root", yaml_dir:const char* = 0, fixup:IYamlFixup* = 0, resolveMergeKeys:bool = true) Dev
        buildHierarchy(in:std::istream&, root_name:const char* = 0, yaml_dir:const char* = 0, fixup:IYamlFixup* = 0, resolveMergeKeys:bool = true) Dev
        buildHierarchy(yaml:YamlNode, root_name:const char* = 0, fixup:IYamlFixup* = 0, resolveMergeKeys:bool = true) Dev
        loadYaml(in:std::istream&) YamlNode
        setSchemaVersion(yaml:YamlNode, maj:int = -1, min:int = -1, rev:int = -1) void
        startHierarchy(Dev) Path
        resolveMergeKeys(YamlNode) void
        getNumberOfUnrecognizedKeys() unsigned long
    }

    class IDev {
        
    }

    class Dev {
        <<typedef>>
    }

}
IField *-- Cacheable
IField <.. Field : sharedptr
IIntField <.. IntField : sharedptr
IDev <.. Dev : sharedptr
IDev --|> IField : virtual
IDev --|> IHub : virtual
IField --|> IEntry : virtual
IIntField --|> IField : virtual

namespace cpsw_entry.h {
    class Visitor {

    }
    class CEntryImpl {
        <<interface>>
        +dumpYamlPart(YAML::Node &)* void
        +dumpMyConfigToYaml(Path, YAML::Node &)* uint64_t
        +loadMyConfigFromYaml(Path, YAML::Node &)* uint64_t
        +processYamlConfig(Path, YAML::Node &, bool)* uint64_t
    }

    class EntryImpl {
        <<typedef>>
    }

    class CDevImpl {

    }

    class ConstDevImpl {
        <<typedef>>
    }

    class IEntryAdapterKey {

    }

    class EntryAdapt {
        <<typedef>>
    }
}

namespace cpsw_entry_adapt.h {
    class IEntryAdapt {
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
}
IEntryAdapt ..|> IVal_Base: implements
IEntryAdapt --|> CShObj
CEntryImpl <.. EntryImpl : sharedptr
CDevImpl <.. ConstDevImpl : const sharedptr
IEntryAdapt <.. EntryAdapt : sharedptr
CEntryImpl --|> IField: virtual
CEntryImpl --|> CShObj
CEntryImpl --|> CYamlSupportBase
```
