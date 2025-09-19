```mermaid
classDiagram
    direction BT

    class CEntryImpl {
        <<class>>
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

    class `cpsw_shared_obj.h : Key`:::cpsw_shared_obj_h {
        <<nested class>>
        -good_ : bool
        -friend class CShObj
        -Key()
        -Key &operator=(const Key &orig)
        +Key(Key &orig)
        +isValid() : bool
    }

    class `cpsw_api_builder.h : IDev`:::cpsw_api_builder_h {
        <<interface>>
        +DFLT_NELMS unsigned$
        
        +addAtAddress(Field f, unsigned nelms)* void
        +~IDev()
        +create(const char* name, uint64_t size)$ Dev
        +startUp()* void
        +isStarted()* const int
        +getRootDev()$ Dev
    }
    
    class `cpsw_hub.h : CDevImpl`:::cpsw_hub_h {
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
    
    class MyChildren:::cpsw_hub_h {
        <<typedef>>
        std::map&lt;const char*, AddressImpl, StrCmp>
    }
    
    class PrioList:::cpsw_hub_h {
        <<typedef>>
        std::list~AddressImpl~
    }
    
    class const_iterator:::cpsw_hub_h {
        <<typedef>>
        MyChildren::const_iterator
    }
    
    `cpsw_hub.h : CDevImpl` *-- MyChildren : contains
    `cpsw_hub.h : CDevImpl` *-- PrioList : contains
    `cpsw_hub.h : CDevImpl` ..> const_iterator : defines
    `cpsw_hub.h : CDevImpl` --|> `cpsw_entry.h : CEntryImpl` : inherits
    `cpsw_hub.h : CDevImpl` ..|> `cpsw_api_builder.h : IDev` : implements
    `cpsw_api_builder.h : IDev` ..|> IHub : implements
    `cpsw_api_builder.h : IDev` ..|> IField : implements

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
    
    class CShObj {
        <<class>>
    }
    
    class CYamlSupportBase {
        <<class>>
    }
    
    class `cpsw_entry.h : CEntryImpl`:::cpsw_entry_h_cc {
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
    
    class `cpsw_entry.cc : CUniqueHandle`:::cpsw_entry_h_cc {
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
    
    class UniqueHandle:::cpsw_entry_h_cc {
        <<typedef>>
        shared_ptr~CUniqueHandle~
    }
    
    class CUniqueListHead:::cpsw_entry_h_cc {
        <<nested class>>
        #CUniqueHandle* n_
        +CUniqueListHead()
        +getNext() CUniqueHandle&ast;
        +setNext(CUniqueHandle* n) void
        +~CUniqueListHead()
    }
    
    class Key {
        <<class>>
    }
    
    class YamlState {
        <<class>>
    }
    
    class Cacheable {
        <<class>>
    }
    
    class CMtxLazy {
        <<class>>
    }
    
    class Path {
        <<class>>
    }
    
    class ConstPath {
        <<class>>
    }
    
    class IVisitor {
        <<interface>>
    }
    
    class RecursionOrder {
        <<enum/class>>
    }
    
    class EntryImpl {
        <<class>>
    }
    
    class Hub {
        <<class>>
    }
    
    class ConstDevImpl {
        <<class>>
    }
    
    class ConstShObj {
        <<class>>
    }
    
    class IEntryAdapterKey {
        <<class>>
    }
    
    class EntryAdapt {
        <<class>>
    }

    `cpsw_entry.h : CEntryImpl` ..|> IField : implements
    `cpsw_entry.h : CEntryImpl` --|> `cpsw_shared_obj.h : CShObj` : inherits
    `cpsw_entry.h : CEntryImpl` --|> CYamlSupportBase : inherits
    `cpsw_entry.h : CEntryImpl` *-- `cpsw_entry.cc : CUniqueHandle` : public nested class
    `cpsw_entry.h : CEntryImpl` *-- CUniqueListHead : private nested class
    `cpsw_entry.h : CEntryImpl` ..> UniqueHandle : defines
    `cpsw_entry.cc : CUniqueHandle` --|> CUniqueListHead : inherits
    UniqueHandle ..> `cpsw_entry.cc : CUniqueHandle` : wraps
    `cpsw_shared_obj.h : CShObj` *-- `cpsw_shared_obj.h : Key` : protected nested class
    `cpsw_shared_obj.h : Key` *-- `cpsw_shared_obj.h : CShObj` : friend class

%% Color schemes
classDef cpsw_api_builder_h fill:#CCFFE5
classDef cpsw_entry_h_cc fill:#CCFFFF
classDef cpsw_hub_h fill:#FFFFCC
classDef cpsw_shared_obj_h fill:#E5CCFF
```
