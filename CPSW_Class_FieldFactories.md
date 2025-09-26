```mermaid
---
config:
  look: neo
  layout: elk
---

classDiagram
    direction BT
    
    class IntEntryImpl:::factory {
        <<typedef>>
        shared_ptr~CIntEntryImpl~
    }

    class IIntField {
        <<interface>>
    }
    
    class CIntEntryImpl {
        <<class>>
        +DFLT_CONFIG_PRIO_RW int$
        +DFLT_CONFIG_BASE int$
        +DFLT_ENCODING Encoding$
        
        -bool isSigned_
        -int lsBit_
        -uint64_t sizeBits_
        -Mode mode_
        -unsigned wordSwap_
        -Encoding encoding_
        -Enum enum_
        -int configBase_
        
        +CIntEntryImpl(Key& k, const char* name, uint64_t sizeBits, bool isSigned, int lsBit, Mode mode, unsigned wordSwap, Enum enm)
        +CIntEntryImpl(Key& k, YamlState& n)
        +CIntEntryImpl(const CIntEntryImpl& orig, Key& k)
        +_getClassName()$ const char*
        +getClassName() const char&ast;
        +dumpYamlPart(YAML::Node& n) const void
        +dumpMyConfigToYaml(Path p, YAML::Node& n) const uint64_t
        +loadMyConfigFromYaml(Path p, YAML::Node& n) const uint64_t
        +clone(Key& k) CIntEntryImpl&ast;
        +isSigned() const bool
        +getLsBit() const int
        +getSizeBits() const uint64_t
        +getWordSwap() const unsigned
        +getEncoding() const Encoding
        +getConfigBase() const int
        +getMode() const Mode
        +getEnum() const Enum
        +setSigned(Key& k, bool v) void
        +setLsBit(Key& k, int v) void
        +setSizeBits(Key& k, uint64_t v) void
        +setWordSwap(Key& k, unsigned v) void
        +setMode(Key& k, Mode v) void
        +setEnum(Key& k, Enum v) void
        +setConfigBase(int base) void
        +setEncoding(Encoding enc) void
        +postHook(ConstShObj obj) void
        +createAdapter(IEntryAdapterKey& key, ConstPath path, const std::type_info& type) const EntryAdapt
        
        #getDefaultConfigPrio() const int
        -checkArgs() void
    }
    
    class CBuilder {
        <<nested class>>
        -std::string name_
        -uint64_t sizeBits_
        -bool isSigned_
        -int lsBit_
        -Mode mode_
        -int configPrio_
        -int configBase_
        -unsigned wordSwap_
        -Encoding encoding_
        -Enum enum_
        
        +CBuilder(Key& k)
        +CBuilder(CBuilder& orig, Key& k)
        +clone(Key& k) CBuilder&ast;
        +init() void
        +name(const char* n) Builder
        +sizeBits(uint64_t bits) Builder
        +isSigned(bool signed) Builder
        +lsBit(int bit) Builder
        +mode(Mode m) Builder
        +configPrio(int prio) Builder
        +configBase(int base) Builder
        +wordSwap(unsigned swap) Builder
        +encoding(Encoding enc) Builder
        +setEnum(Enum e) Builder
        +reset() Builder
        +build() IntField
        +build(const char* name) IntField
        +clone() Builder
        +create()$ Builder
    }
    
    class IBuilder {
        <<interface>>
    }
    
    class `cpsw_shared_obj.h : CShObj`:::cpsw_shared_obj_h {
        <<class>>
    }
    
    class BuilderImpl {
        <<typedef>>
        shared_ptr~CBuilder~
    }
    
    class Encoding {
        <<public typedef>>
        IScalVal_Base::Encoding
    }
    
    class TODO_Mode {
        <<enum/class>>
    }
    
    class TODO_Enum {
        <<class>>
    }

    class EntryImpl:::factory {
        <<typedef>>
        shared_ptr~CEntryImpl~
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
    
    `cpsw_entry.h : CEntryImpl` ..|> `cpsw_api_builder.h : IField` : implements
    `cpsw_entry.h : CEntryImpl` --|> `cpsw_shared_obj.h : CShObj` : inherits
    `cpsw_entry.h : CEntryImpl` --|> `cpsw_yaml.h : CYamlSupportBase` : inherits
    `cpsw_entry.h : CEntryImpl` *-- `cpsw_entry.cc : CUniqueHandle` : public nested class
    `cpsw_entry.h : CEntryImpl` *-- CUniqueListHead : private nested class
    `cpsw_entry.h : CEntryImpl` ..> UniqueHandle : defines
    EntryImpl ..> `cpsw_entry.h : CEntryImpl` : wraps
    IntEntryImpl ..> CIntEntryImpl : wraps
    CIntEntryImpl --|> `cpsw_entry.h : CEntryImpl` : inherits
    CIntEntryImpl ..|> IIntField : implements
    CIntEntryImpl *.. CBuilder : public nested class
    CIntEntryImpl *.. Encoding : defines
    CBuilder --|> `cpsw_shared_obj.h : CShObj` : inherits
    CBuilder ..|> IBuilder : implements
    CBuilder *.. BuilderImpl : nested typedef
    CBuilder -- TODO_Mode
    CBuilder -- Encoding
    CBuilder -- TODO_Enum
    CIntEntryImpl -- TODO_Mode
    CIntEntryImpl -- TODO_Enum
    CIntEntryImpl -- Encoding

%% Color schemes
classDef cpsw_api_user_h fill:#CCFFE5
classDef cpsw_api_builder_h fill:#99FFCC
classDef cpsw_entry_h_cc fill:#CCFFFF
classDef cpsw_hub_h fill:#FFFFCC
classDef cpsw_shared_obj_h fill:#E5CCFF
classDef cpsw_yaml_h fill:#FFE5CC
classDef factory fill:#FF6666
```
