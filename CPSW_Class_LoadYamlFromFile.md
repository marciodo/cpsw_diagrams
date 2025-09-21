```mermaid
---
config:
  look: neo
  layout: elk
  theme: neutral
---

classDiagram
    direction BT
    
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
    
    class `cpsw_api_builder.h : Dev`:::cpsw_api_builder_h {
        <<typedef>>
        shared_ptr~IDev~
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
    `cpsw_api_builder.h : IDev` ..|> `cpsw_api_user.h : IHub` : implements
    `cpsw_api_builder.h : IDev` ..|> `cpsw_api_builder.h : IField` : implements

    class `cpsw_api_builder.h : IField`:::cpsw_api_builder_h {
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
    
    class Cacheable:::cpsw_api_builder_h {
        <<enumeration>>
        UNKNOWN_CACHEABLE = 0
        NOT_CACHEABLE
        WT_CACHEABLE
        WB_CACHEABLE
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

    class `cpsw_api_builder.h : IVisitable`:::cpsw_api_builder_h {
        <<interface>>
        +DEPTH_INDEFINITE int$
        +DEPTH_NONE int$
        
        +accept(IVisitor* v, RecursionOrder order, int recursionDepth)* void
        +~IVisitable()
    }
    
    class RecursionOrder:::cpsw_api_builder_h {
        <<enumeration>>
        RECURSE_DEPTH_FIRST = true
        RECURSE_DEPTH_AFTER = false
    }

    class `cpsw_api_user.h : IYamlSupportBase`:::cpsw_api_user_h {
        <<interface>>
        +dumpYaml(&n:YAML::Node)* void
        +MIN_SUPPORTED_SCHEMA=3 : const int$
        +MAX_SUPPORTED_SCHEMA=3 : const int$
    }
    
    class `cpsw_yaml.h : CYamlSupportBase`:::cpsw_yaml_h {
        +dumpYamlPart(YAML::Node& node) const void
        +_getClassName()$ const char*
        +getClassName() const char&ast;
        +overrideNode(YamlState& orig)$ void
        +setClassName(YAML::Node& node) const void
        +dumpYaml(YAML::Node& node) const void
    }
    
    class `cpsw_api_user.h : IHub`:::cpsw_api_user_h {
        <<interface>>
        +findByName(const char* path)* const Path
        +getChild(const char* name)* const Child
        +getChildren()* const Children
        +~IHub()
    }

    class `cpsw_api_builder.h : IYamlSupport`:::cpsw_api_builder_h {
        <<namespace>>
        +dumpYamlFile(Entry top, const char* filename, const char* root_entry_name)$ void
        +buildHierarchy(const char* yaml_name, const char* root_name, const char* yaml_dir, IYamlFixup* fixup, bool resolveMergeKeys)$ Dev
        +buildHierarchy(std::istream& in, const char* root_name, const char* yaml_dir, IYamlFixup* fixup, bool resolveMergeKeys)$ Dev
        +buildHierarchy(YamlNode yaml, const char* root_name, IYamlFixup* fixup, bool resolveMergeKeys)$ Dev
        +loadYaml(std::istream& in)$ YamlNode
        +setSchemaVersion(YamlNode yaml, int maj, int min, int rev)$ void
        +startHierarchy(Dev device)$ Path
        +resolveMergeKeys(YamlNode node)$ void
        +getNumberOfUnrecognizedKeys()$ unsigned long
    }

    class `cpsw_yaml.h : IYamlFactoryBase&lt;T>`:::cpsw_yaml_h {
        -Registry registry_
        -const char* className_
        
        +IYamlFactoryBase(const char* className, Registry r)
        +makeItem(YamlState& node)* T
        +getRegistry() Registry
        +~IYamlFactoryBase()
    }

    class Registry:::cpsw_yaml_h {
        <<typedef>>
        shared_ptr~IYamlTypeRegistry~T~~
    }
    
    class `cpsw_yaml.h : CYamlFieldFactoryBase`:::cpsw_yaml_h {
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
    
    class FieldRegistry:::cpsw_yaml_h {
        <<typedef>>
        IYamlFactoryBase~Field~::Registry
    }

    class `cpsw_yaml.h : IYamlTypeRegistry&ltT>`:::cpsw_yaml_h {
        +makeItem(YamlState& state)* T
        +extractClassName(std::vector~std::string~* svec_p, YamlState& state)* void
        +addFactory(const char* className, IYamlFactoryBase~T~* f)* void
        +delFactory(const char* className)* void
        +dumpClasses(std::ostream& os)* void
    }

    class `cpsw_yaml.h : IRegistry`:::cpsw_yaml_h {
        <<interface>>
        +addItem(const char* name, void* item)* void
        +delItem(const char* name)* void
        +getItem(const char* name)* void*
        +dumpItems(std::ostream& os)* void
        +create(bool dynLoad)$ IRegistry*
        +~IRegistry()
    }

    class `cpsw_yaml.h : CYamlTypeRegistry&ltT>`:::cpsw_yaml_h {
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

    `cpsw_yaml.h : CYamlTypeRegistry&ltT>` -- `cpsw_yaml.h : IRegistry`
    `cpsw_yaml.h : CYamlTypeRegistry&ltT>` --|> `cpsw_yaml.h : IYamlTypeRegistry&ltT>` : implements
    `cpsw_yaml.h : IYamlFactoryBase&lt;T>` ..> Registry : defines
    `cpsw_yaml.h : CYamlFieldFactoryBase` --|> `cpsw_yaml.h : IYamlFactoryBase&lt;T>` : (bind)[T->Field]
    `cpsw_yaml.h : CYamlFieldFactoryBase` ..> FieldRegistry : defines
    Registry ..> `cpsw_yaml.h : IYamlTypeRegistry&ltT>` : wraps
    FieldRegistry .. Registry : (bind)[T->Field]
    `cpsw_yaml.h : CYamlSupportBase` ..|> `cpsw_api_user.h : IYamlSupportBase` : implements
    `cpsw_api_user.h : IHub` ..|> `cpsw_api_user.h : IEntry` : implements
    `cpsw_api_builder.h : IVisitable` *-- RecursionOrder : public nested enum
    `cpsw_entry.h : CEntryImpl` ..|> `cpsw_api_builder.h : IField` : implements
    `cpsw_entry.h : CEntryImpl` --|> `cpsw_shared_obj.h : CShObj` : inherits
    `cpsw_entry.h : CEntryImpl` --|> `cpsw_yaml.h : CYamlSupportBase` : inherits
    `cpsw_entry.h : CEntryImpl` *-- `cpsw_entry.cc : CUniqueHandle` : public nested class
    `cpsw_entry.h : CEntryImpl` *-- CUniqueListHead : private nested class
    `cpsw_entry.h : CEntryImpl` ..> UniqueHandle : defines
    `cpsw_entry.cc : CUniqueHandle` --|> CUniqueListHead : inherits
    UniqueHandle ..> `cpsw_entry.cc : CUniqueHandle` : wraps
    `cpsw_api_builder.h : Dev` ..> `cpsw_api_builder.h : IDev` : wraps
    `cpsw_shared_obj.h : CShObj` *-- `cpsw_shared_obj.h : Key` : protected nested class
    `cpsw_shared_obj.h : Key` *-- `cpsw_shared_obj.h : CShObj` : friend class
    `cpsw_api_builder.h : IField` ..|> `cpsw_api_user.h : IEntry` : implements
    `cpsw_api_builder.h : IField` ..|> `cpsw_api_builder.h : IVisitable` : implements
    `cpsw_api_builder.h : IField` *-- Cacheable : nested enum

%% Color schemes
classDef cpsw_api_user_h fill:#CCFFE5
classDef cpsw_api_builder_h fill:#99FFCC
classDef cpsw_entry_h_cc fill:#CCFFFF
classDef cpsw_hub_h fill:#FFFFCC
classDef cpsw_shared_obj_h fill:#E5CCFF
classDef cpsw_yaml_h fill:#FFE5CC
```
