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

namespace cpsw_yaml.h_or_cc {
    class CYamlSupportBase {
        +dumpYamlPart(YAML::Node& node) const void
        +_getClassName()$ const char*
        +getClassName() const char&ast;
        +overrideNode(YamlState& orig)$ void
        +setClassName(YAML::Node& node) const void
        +dumpYaml(YAML::Node& node) const void
    }

    class IYamlFactoryBase~T~ {
        -Registry registry_
        -const char* className_
        
        +IYamlFactoryBase(const char* className, Registry r)
        +makeItem(YamlState& node)* T
        +getRegistry() Registry
        +~IYamlFactoryBase()
    }

    class Registry {
        <<typedef>>
        shared_ptr~IYamlTypeRegistry~T~~
    }
    
        class IRegistry {
        <<interface>>
        +addItem(const char* name, void* item)* void
        +delItem(const char* name)* void
        +getItem(const char* name)* void*
        +dumpItems(std::ostream& os)* void
        +create(bool dynLoad)$ IRegistry*
        +~IRegistry()
    }

    class `CYamlTypeRegistry&ltT>` {
        <<template class>>
        -IRegistry* registry_
        
        +CYamlTypeRegistry(bool dynLoadSupport)
        +~CYamlTypeRegistry()
        +addFactory(const char* className, IYamlFactoryBase~T~* f) void
        +delFactory(const char* className) void
        +extractClassName(std::vector~std::string~* svec_p, YamlState& n) void
        +extractInstantiate(YamlState& n) bool
        +makeItem(YamlState& n) T
        +dumpClasses(std::ostream& os) void
        
        -CYamlTypeRegistry(const CYamlTypeRegistry&)
        -operator=(const CYamlTypeRegistry&) CYamlTypeRegistry
    }

    class CYamlFieldFactoryBase {
        +dispatchMakeField(const YAML::Node& node, const char* root_name)$ Dev
        +loadPreprocessedYaml(std::istream& stream, const char* yaml_dir, bool resolveMergeKeys)$ YAML::Node
        +loadPreprocessedYaml(const char* char_stream, const char* yaml_dir, bool resolveMergeKeys)$ YAML::Node
        +loadPreprocessedYamlFile(const char* file_name, const char* yaml_dir, bool resolveMergeKeys)$ YAML::Node
        +dumpClasses(std::ostream& os)$ void
        +getFieldRegistry_()$ FieldRegistry
        
        #CYamlFieldFactoryBase(const char* typeLabel)
        #addChildren(CEntryImpl& entry, YamlState& state) void
        #addChildren(CDevImpl& dev, YamlState& state) void
    }
    
    class FieldRegistry {
        <<typedef>>
        IYamlFactoryBase~Field~::Registry
    }

    class IYamlTypeRegistry~T~ {
        +makeItem(YamlState& state)* T
        +extractClassName(std::vector~std::string~* svec_p, YamlState& state)* void
        +addFactory(const char* className, IYamlFactoryBase~T~* f)* void
        +delFactory(const char* className)* void
        +dumpClasses(std::ostream& os)* void
    }

    class SYamlState {
        <<struct>>
        -Set unusedKeys
        +SYamlState(YamlState* parent, unsigned index)
        +SYamlState(YamlState* parent, const char* key)
        +SYamlState(YamlState* parent, const char* key, const YAML::Node& node)
        +~SYamlState()
        +resetUnrecognizedKeys()$ void
        +getUnrecognizedKeys()$ unsigned long
        +incUnrecognizedKeys()$ unsigned long
        +keySeen(const char* key) const void
        +purgeKeys() const void
        
        -SYamlState(const SYamlState& orig)
        -operator=(const SYamlState* other) SYamlState&
        -incUnrecognizedKeys(int op)$ unsigned long
        -initSieve() const void
    }

    class UnusedKey {
        <<public nested class>>
        -const char* name_
        -int line_
        -int col_
        
        +UnusedKey(const YAML::Node& key)
        +UnusedKey(const char* k)
        +getName() const char&ast;
        +getLine() const int
        +getColumn() const int
    }

    class UnusedKeyCmp {
        <<public nested struct>>
        +operator()(const UnusedKey& a, const UnusedKey& b) const int
    }

    class `SYamlState::Set` {
        <<private typedef>>
        std::set~UnusedKey, UnusedKeyCmp~
    }

    class `YAML::PNode` {
            <<class>>
            -const PNode* parent_
            -const char* key_
            
            +PNode(const PNode* parent, const char* key)
            +PNode(const PNode* parent, const char* key, const Node& node)
            +PNode(const PNode* parent, unsigned index)
            +~PNode()
            +dump(FILE* f) const void
            +getName() const char&ast;
            +getParent() const PNode&ast;
            +toString() const std::string
            +lookup(const char* key) const PNode
            
            -PNode(const PNode& orig)
            -operator=(const Node& orig_node) PNode&
    }

    class CYamlNode {
        +CYamlNode(const YAML::Node& n)
    }

    class CYamlFieldFactory~T~ {
        <<template class>>
        +CYamlFieldFactory()
        +makeItem(YamlState& state) Field
    }

    class RegistryImpl {
        -Map map_
        -Set failed_
        -bool dynLoad_
        
        +RegistryImpl(bool dynLoad)
        +~RegistryImpl()
        +addItem(const char* name, void* item) void
        +delItem(const char* name) void
        +getItem(const char* name) void&ast;
        +dumpItems(std::ostream& os) void
    }

    class Map {
        <<public typedef>>
        std::map~std::string, void*~
    }
    
    class Member {
        <<public typedef>>
        std::pair~std::string, void*~
    }
    
    class `RegistryImpl::Set` {
        <<public typedef>>
        StrSet
    }

    class StrSet {
        <<typedef>>
        unordered_set~std::string~
    }
}

class `YAML::Node` {
        <<external yaml-cpp>>
    }
    
RegistryImpl --|> IRegistry : implements
RegistryImpl ..> Map : defines
RegistryImpl ..> Member : defines
RegistryImpl ..> `RegistryImpl::Set` : defines
`RegistryImpl::Set` ..> StrSet : is
CYamlFieldFactory~T~ --|> CYamlFieldFactoryBase : inherits
`YAML::PNode` -- `YAML::PNode` : parent_
`YAML::PNode` --|> `YAML::Node` : inherits
SYamlState --|> `YAML::PNode` : inherits
note for `YAML::PNode` "YAML is also a namespace in cpsw_yam.h"
SYamlState *-- `SYamlState::Set` : contains
SYamlState ..> UnusedKeyCmp : defines
SYamlState ..> UnusedKey : defines
`CYamlTypeRegistry&ltT>` --|> IYamlTypeRegistry~T~ : implements
`CYamlTypeRegistry&ltT>` -- IRegistry
CYamlSupportBase ..|> IYamlSupportBase : implements
IYamlFactoryBase~T~ ..> Registry : defines
CYamlFieldFactoryBase --|> IYamlFactoryBase~Field~ : (bind)[T->Field]
FieldRegistry .. Registry : (bind)[T->Field]
CYamlFieldFactoryBase ..> FieldRegistry : defines
Registry ..> IYamlTypeRegistry~T~ : wraps

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

CShObj *-- Key : protected nested class
Key *-- CShObj : friend class

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
        +~IEntry(void)
    }

    class IHub {
        <<interface>>
        +findByName(const char* path)* const Path
        +getChild(const char* name)* const Child
        +getChildren()* const Children
        +~IHub()
    }

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
IHub ..|> IEntry: implements
IVal_Base *-- Mode
IVal_Base *-- Encoding
IChild --|> IEntry

namespace cpsw_api_builder.h {
    
    class IVisitable {
        <<interface>>
        +DEPTH_INDEFINITE int$
        +DEPTH_NONE int$
        
        +accept(IVisitor* v, RecursionOrder order, int recursionDepth)* void
        +~IVisitable()
    }
    
    class RecursionOrder {
        <<enumeration>>
        RECURSE_DEPTH_FIRST = true
        RECURSE_DEPTH_AFTER = false
    }
    
    class IField {
        <<interface>>
        +DFLT_SIZE uint64_t$
        +DFLT_CACHEABLE Cacheable$
        
        +getCacheable()* const Cacheable
        +setCacheable(Cacheable c)* void
        +setDescription(const char* desc)* void
        +setDescription(const std::string& desc)* void
        +~IField()
        +getSelf()* EntryImpl
        +create(const char* name, uint64_t size)$ Field
    }

    class Field {
        <<typedef>>
        shared_ptr~IField~
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
        <<interface>>
        +DFLT_NELMS unsigned$
        
        +addAtAddress(Field f, unsigned nelms)* void
        +~IDev()
        +create(const char* name, uint64_t size)$ Dev
        +startUp()* void
        +isStarted()* const int
        +getRootDev()$ Dev
    }

    class Dev {
        <<typedef>>
        shared_ptr~IDev~
    }

    class YamlState {
        <<typedef>>
        const struct SYamlState
    }

    class YamlNode {
        <<typedef>>
        shared_ptr~CYamlNode~
    }

}

CYamlNode --|> `YAML::Node` : inherits
YamlNode ..> CYamlNode : wraps
YamlState ..> SYamlState : aliases
IVisitable *-- RecursionOrder : public nested enum
IField ..|> IVisitable : implements
IField *-- Cacheable
IField <.. Field : wraps
IIntField <.. IntField : sharedptr
IDev <.. Dev : wraps
IDev ..|> IField : implements
IDev ..|> IHub : implements
IField --|> IEntry : virtual
IIntField --|> IField : virtual

namespace cpsw_entry.h {
    class Visitor {

    }
    class CEntryImpl {
                +CONFIG_PRIO_OFF int$
        +DFLT_CONFIG_PRIO int$
        +DFLT_CONFIG_PRIO_DEV int$
        +DFLT_POLL_SECS() double$
        
        -std::string name_
        -std::string description_
        -bool singleInterfaceOnly_
        -Cacheable cacheable_
        -int configPrio_
        -bool configPrioSet_
        -double pollSecs_
        -bool locked_
        -CMtxLazy uniqueListMtx_
        -CUniqueListHead uniqueListHead_
        -int started_
        #uint64_t size_
        
        +CEntryImpl(Key& k, const char* name, uint64_t size)
        +CEntryImpl(Key& k, YamlState& n)
        +~CEntryImpl()
        +dumpYamlPart(YAML::Node& node) const void
        +dumpMyConfigToYaml(Path p, YAML::Node& node) const uint64_t
        +loadMyConfigFromYaml(Path p, YAML::Node& node) const uint64_t
        +processYamlConfig(Path p, YAML::Node& node, bool flag) const uint64_t
        +_getClassName()$ const char*
        +getClassName() const char&ast;
        +clone(Key& k) CEntryImpl&ast;
        +getName() const char&ast;
        +getDescription() const char&ast;
        +getPollSecs() const double
        +setDescription(const char* desc) void
        +setDescription(const std::string& desc) void
        +getSize() const uint64_t
        +getCacheable() const Cacheable
        +getConfigPrio() const int
        +setCacheable(Cacheable cacheable) void
        +setConfigPrio(int configPrio) void
        +setPollSecs(double pollSecs) void
        +setLocked() void
        +getLocked() bool
        +postHook(ConstShObj obj) void
        +accept(IVisitor* v, RecursionOrder order, int recursionDepth) void
        +getSelf() EntryImpl
        +getConstSelf() const EntryImpl
        +isHub() const Hub
        +isConstDevImpl() const ConstDevImpl
        +singleInterfaceOnly() const bool
        +getUniqueHandle(IEntryAdapterKey& key, ConstPath p) const UniqueHandle
        +createAdapter(IEntryAdapterKey& key, ConstPath p, const std::type_info& interfaceType) const EntryAdapt
        +dump(FILE* file) const void
        +dump() const void
        
        #CEntryImpl(const CEntryImpl& ei, Key& k)
        #getDefaultConfigPrio() const int
        #getDefaultPollSecs() const double
        #_createAdapter~ADAPT, IMPL~(const IMPL* helper, ConstPath p) const EntryAdapt
        #isInterface~INTRF~(const std::type_info& interfaceType)$ bool
        
        -CEntryImpl(const CEntryImpl&)
        -operator=(const CEntryImpl&) CEntryImpl&
        -checkArgs() void
    }

    class CUniqueHandle {
        <<nested class>>
        -CUniqueListHead* p_
        -CMtx* mtx_
        -ConstPath path_
        
        +CUniqueHandle(CMtx* m, ConstPath path)
        +add_unguarded(CUniqueListHead* h) void
        +del_unguarded() void
        +getConstPath() ConstPath
        +~CUniqueHandle()
    }
    
    class UniqueHandle {
        <<typedef>>
        shared_ptr~CUniqueHandle~
    }
    
    class CUniqueListHead {
        <<nested class>>
        #CUniqueHandle* n_
        +CUniqueListHead()
        +getNext() CUniqueHandle&ast;
        +setNext(CUniqueHandle* n) void
        +~CUniqueListHead()
    }

    class EntryImpl {
        <<typedef>>
    }

    class ConstDevImpl {
        <<typedef>>
        shared_ptr~const CDevImpl~
    }

    class IEntryAdapterKey {

    }

    class EntryAdapt {
        <<typedef>>
    }
}

CEntryImpl *-- CUniqueHandle : public nested class
CEntryImpl *-- CUniqueListHead : private nested class
CEntryImpl ..> UniqueHandle : defines
CUniqueHandle --|> CUniqueListHead : inherits
UniqueHandle ..> CUniqueHandle : wraps

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
CDevImpl <.. ConstDevImpl : uses
IEntryAdapt <.. EntryAdapt : sharedptr
CEntryImpl ..|> IField : implements
CEntryImpl --|> CShObj : inherits
CEntryImpl --|> CYamlSupportBase : inherits

namespace cpsw_hub.h_or_cc {
class CDevImpl {
        -int started_
        -MyChildren children_
        -PrioList configPrioList_
        
        +CDevImpl(Key& k, const char* name, uint64_t size)
        +CDevImpl(Key& k, YamlState& ypath)
        +~CDevImpl()
        +dumpYamlPart(YAML::Node& node) const void
        +_getClassName()$ const char*
        +getClassName() const char&ast;
        +clone(Key& k) CDevImpl&ast;
        +postHook(ConstShObj orig) void
        +addAtAddress(Field child, unsigned nelms) void
        +doAddAtAddress~T~(Field child, YamlState& ypath) shared_ptr~T~
        +addAtAddress(Field child, YamlState& ypath) void
        +findByName(const char* s) const Path
        +getChild(const char* name) const Child
        +getAddress(const char* name) const Address
        +accept(IVisitor* v, RecursionOrder order, int recursionDepth) void
        +getChildren() const Children
        +isHub() const Hub
        +isConstDevImpl() const ConstDevImpl
        +begin() const const_iterator
        +end() const const_iterator
        +isStarted() const int
        +startUp() void
        +processYamlConfig(Path p, YAML::Node& node, bool flag) const uint64_t
        
        #add(AddressImpl a, Field child) void
        #getAKey() IAddress::AKey
        #CDevImpl(const CDevImpl& orig, Key& k)
        #getDefaultConfigPrio() const int
    }

    class MyChildren {
        <<typedef>>
        std::map&lt;const char*, AddressImpl, StrCmp>
    }
    
    class PrioList {
        <<typedef>>
        std::list~AddressImpl~
    }
    
    class const_iterator {
        <<typedef>>
        MyChildren::const_iterator
    }
}

CDevImpl *-- MyChildren : contains
CDevImpl *-- PrioList : contains
CDevImpl ..> const_iterator : defines
CDevImpl --|> CEntryImpl : inherits
CDevImpl ..|> IDev : implements
```
