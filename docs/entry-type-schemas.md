# Entry Type Schemas for Memetic Messenger

## Core Data Structures

Based on our design specifications, here are the entry types for each applet:

### 1. Clock Applet

**Timestamp Entry:**
```rust
#[derive(Clone, Debug, PartialEq, Serialize, Deserialize, SerializedBytes)]
pub struct Timestamp {
    pub unix_time: u64,
    pub timezone: String,
    pub precision: TimePrecision, // Second, Millisecond, Nanosecond
}

#[derive(Clone, Debug, PartialEq, Serialize, Deserialize)]
pub enum TimePrecision {
    Second,
    Millisecond,
    Nanosecond,
}
```

**Schedule Entry:**
```rust
#[derive(Clone, Debug, PartialEq, Serialize, Deserialize, SerializedBytes)]
pub struct Schedule {
    pub title: String,
    pub start_time: Timestamp,
    pub end_time: Option<Timestamp>,
    pub recurrence: Option<RecurrencePattern>,
    pub creator: AgentPubKey,
}

#[derive(Clone, Debug, PartialEq, Serialize, Deserialize)]
pub struct RecurrencePattern {
    pub frequency: Frequency, // Daily, Weekly, Monthly, etc.
    pub interval: u32,
    pub end_date: Option<Timestamp>,
}
```

### 2. Calendar Applet

**Event Entry:**
```rust
#[derive(Clone, Debug, PartialEq, Serialize, Deserialize, SerializedBytes)]
pub struct Event {
    pub title: String,
    pub description: Option<String>,
    pub start_time: Timestamp,
    pub end_time: Timestamp,
    pub location: Option<String>,
    pub attendees: Vec<AgentPubKey>,
    pub creator: AgentPubKey,
    pub event_type: EventType,
}

#[derive(Clone, Debug, PartialEq, Serialize, Deserialize)]
pub enum EventType {
    Meeting,
    Personal,
    Social,
    Work,
    Other(String),
}
```

### 3. Identity Applet

**Profile Entry:**
```rust
#[derive(Clone, Debug, PartialEq, Serialize, Deserialize, SerializedBytes)]
pub struct Profile {
    pub display_name: String,
    pub bio: Option<String>,
    pub avatar: Option<String>, // Content hash or URL
    pub fields: HashMap<String, String>, // Custom fields
    pub visibility: ProfileVisibility,
    pub created_at: Timestamp,
    pub updated_at: Timestamp,
}

#[derive(Clone, Debug, PartialEq, Serialize, Deserialize)]
pub enum ProfileVisibility {
    Public,
    Private,
    Contacts,
}
```

**Persona Entry:**
```rust
#[derive(Clone, Debug, PartialEq, Serialize, Deserialize, SerializedBytes)]
pub struct Persona {
    pub name: String,
    pub context: PersonaContext, // Professional, Personal, etc.
    pub profile_hash: ActionHash, // Link to Profile
    pub active: bool,
    pub created_at: Timestamp,
}

#[derive(Clone, Debug, PartialEq, Serialize, Deserialize)]
pub enum PersonaContext {
    Professional,
    Personal,
    Social,
    Creative,
    Other(String),
}
```

### 4. Address Book Applet

**Contact Entry:**
```rust
#[derive(Clone, Debug, PartialEq, Serialize, Deserialize, SerializedBytes)]
pub struct Contact {
    pub agent_key: AgentPubKey,
    pub display_name: String,
    pub nickname: Option<String>,
    pub relationship: ContactRelationship,
    pub groups: Vec<String>,
    pub notes: Option<String>,
    pub added_at: Timestamp,
    pub last_interaction: Option<Timestamp>,
}

#[derive(Clone, Debug, PartialEq, Serialize, Deserialize)]
pub enum ContactRelationship {
    Friend,
    Family,
    Colleague,
    Acquaintance,
    Other(String),
}
```

### 5. Membrane Manager Applet

**Membrane Entry:**
```rust
#[derive(Clone, Debug, PartialEq, Serialize, Deserialize, SerializedBytes)]
pub struct Membrane {
    pub name: String,
    pub description: Option<String>,
    pub membrane_type: MembraneType,
    pub members: Vec<AgentPubKey>,
    pub admins: Vec<AgentPubKey>,
    pub rules: MembraneRules,
    pub created_by: AgentPubKey,
    pub created_at: Timestamp,
}

#[derive(Clone, Debug, PartialEq, Serialize, Deserialize)]
pub enum MembraneType {
    Private,
    Group,
    Public,
}

#[derive(Clone, Debug, PartialEq, Serialize, Deserialize)]
pub struct MembraneRules {
    pub join_policy: JoinPolicy,
    pub post_policy: PostPolicy,
    pub visibility: MembraneVisibility,
}
```

### 6. Communication Applet

**Message Entry:**
```rust
#[derive(Clone, Debug, PartialEq, Serialize, Deserialize, SerializedBytes)]
pub struct Message {
    pub content: MessageContent,
    pub sender: AgentPubKey,
    pub recipients: Vec<AgentPubKey>, // Empty for public messages
    pub membrane_hash: Option<ActionHash>, // Which membrane this belongs to
    pub thread_root: Option<ActionHash>, // Root message of thread
    pub reply_to: Option<ActionHash>, // Direct reply reference
    pub timestamp: Timestamp,
    pub message_type: MessageType,
}

#[derive(Clone, Debug, PartialEq, Serialize, Deserialize)]
pub struct MessageContent {
    pub text: Option<String>,
    pub attachments: Vec<ActionHash>, // References to files
    pub mentions: Vec<AgentPubKey>,
    pub hashtags: Vec<String>,
}

#[derive(Clone, Debug, PartialEq, Serialize, Deserialize)]
pub enum MessageType {
    Direct,
    Group,
    Public,
    Broadcast,
}
```

### 7. Frames (Files) Applet

**File Entry:**
```rust
#[derive(Clone, Debug, PartialEq, Serialize, Deserialize, SerializedBytes)]
pub struct File {
    pub name: String,
    pub content_type: String,
    pub size: u64,
    pub content_hash: String, // Hash of the actual file content
    pub chunks: Vec<ActionHash>, // For large files split into chunks
    pub metadata: FileMetadata,
    pub uploaded_by: AgentPubKey,
    pub uploaded_at: Timestamp,
}

#[derive(Clone, Debug, PartialEq, Serialize, Deserialize)]
pub struct FileMetadata {
    pub width: Option<u32>, // For images
    pub height: Option<u32>, // For images
    pub duration: Option<u32>, // For videos/audio
    pub tags: Vec<String>,
    pub description: Option<String>,
}
```

**Collection Entry:**
```rust
#[derive(Clone, Debug, PartialEq, Serialize, Deserialize, SerializedBytes)]
pub struct Collection {
    pub name: String,
    pub description: Option<String>,
    pub collection_type: CollectionType,
    pub items: Vec<ActionHash>, // Links to files or other entries
    pub owner: AgentPubKey,
    pub created_at: Timestamp,
    pub updated_at: Timestamp,
}

#[derive(Clone, Debug, PartialEq, Serialize, Deserialize)]
pub enum CollectionType {
    Media,
    Memes,
    Stickers,
    Documents,
    Other(String),
}
```

### 8. Dashboard Applet

**Dashboard View:**
```rust
#[derive(Clone, Debug, PartialEq, Serialize, Deserialize, SerializedBytes)]
pub struct DashboardView {
    pub name: String,
    pub widgets: Vec<Widget>,
    pub layout: LayoutConfig,
    pub owner: AgentPubKey,
    pub created_at: Timestamp,
    pub updated_at: Timestamp,
}

#[derive(Clone, Debug, PartialEq, Serialize, Deserialize)]
pub struct Widget {
    pub widget_type: WidgetType,
    pub position: Position,
    pub size: Size,
    pub config: serde_json::Value, // Widget-specific configuration
}

#[derive(Clone, Debug, PartialEq, Serialize, Deserialize)]
pub enum WidgetType {
    Timeline,
    Clock,
    Calendar,
    Messages,
    Contacts,
    Files,
    Custom(String),
}
```

## Link Types

```rust
// Between applets
pub const LINK_MESSAGE_TO_FILE: &str = "message->file";
pub const LINK_EVENT_TO_ATTENDEE: &str = "event->attendee";
pub const LINK_CONTACT_TO_MEMBRANE: &str = "contact->membrane";
pub const LINK_PERSONA_TO_PROFILE: &str = "persona->profile";

// Within applets
pub const LINK_THREAD_TO_MESSAGE: &str = "thread->message";
pub const LINK_COLLECTION_TO_FILE: &str = "collection->file";
pub const LINK_MEMBRANE_TO_MEMBER: &str = "membrane->member";
```

## Validation Rules

Each entry type will have validation functions to ensure:
- Required fields are present
- Data formats are correct
- Agent permissions are respected
- Business logic constraints are met

---

*These schemas provide the foundation for implementing all 8 applets with proper data structures and relationships.*