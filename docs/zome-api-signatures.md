# Zome API Function Signatures

## Architecture Overview

**Integrity Zome:** Contains entry type definitions and validation rules
**Coordinator Zomes:** One per applet, containing the business logic and API functions

## 1. Clock Coordinator Zome

```rust
// Timestamp functions
#[hdk_extern]
pub fn create_timestamp(timestamp: Timestamp) -> ExternResult<Record>;

#[hdk_extern]
pub fn get_timestamp(action_hash: ActionHash) -> ExternResult<Option<Record>>;

#[hdk_extern]
pub fn get_current_time() -> ExternResult<Timestamp>;

// Schedule functions
#[hdk_extern]
pub fn create_schedule(schedule: Schedule) -> ExternResult<Record>;

#[hdk_extern]
pub fn update_schedule(
    original_action_hash: ActionHash,
    updated_schedule: Schedule,
) -> ExternResult<Record>;

#[hdk_extern]
pub fn delete_schedule(action_hash: ActionHash) -> ExternResult<ActionHash>;

#[hdk_extern]
pub fn get_my_schedules() -> ExternResult<Vec<Record>>;

#[hdk_extern]
pub fn get_schedules_by_time_range(
    start: Timestamp,
    end: Timestamp,
) -> ExternResult<Vec<Record>>;
```

## 2. Calendar Coordinator Zome

```rust
// Event functions
#[hdk_extern]
pub fn create_event(event: Event) -> ExternResult<Record>;

#[hdk_extern]
pub fn update_event(
    original_action_hash: ActionHash,
    updated_event: Event,
) -> ExternResult<Record>;

#[hdk_extern]
pub fn delete_event(action_hash: ActionHash) -> ExternResult<ActionHash>;

#[hdk_extern]
pub fn get_event(action_hash: ActionHash) -> ExternResult<Option<Record>>;

#[hdk_extern]
pub fn get_my_events() -> ExternResult<Vec<Record>>;

#[hdk_extern]
pub fn get_events_by_date_range(
    start: Timestamp,
    end: Timestamp,
) -> ExternResult<Vec<Record>>;

#[hdk_extern]
pub fn invite_to_event(
    event_hash: ActionHash,
    invitee: AgentPubKey,
) -> ExternResult<()>;

#[hdk_extern]
pub fn respond_to_event_invitation(
    event_hash: ActionHash,
    response: InvitationResponse,
) -> ExternResult<()>;

// Integration with Clock zome
#[hdk_extern]
pub fn sync_with_schedules() -> ExternResult<Vec<Record>>;
```

## 3. Identity Coordinator Zome

```rust
// Profile functions
#[hdk_extern]
pub fn create_profile(profile: Profile) -> ExternResult<Record>;

#[hdk_extern]
pub fn update_profile(
    original_action_hash: ActionHash,
    updated_profile: Profile,
) -> ExternResult<Record>;

#[hdk_extern]
pub fn get_my_profile() -> ExternResult<Option<Record>>;

#[hdk_extern]
pub fn get_profile(agent: AgentPubKey) -> ExternResult<Option<Record>>;

// Persona functions
#[hdk_extern]
pub fn create_persona(persona: Persona) -> ExternResult<Record>;

#[hdk_extern]
pub fn update_persona(
    original_action_hash: ActionHash,
    updated_persona: Persona,
) -> ExternResult<Record>;

#[hdk_extern]
pub fn delete_persona(action_hash: ActionHash) -> ExternResult<ActionHash>;

#[hdk_extern]
pub fn get_my_personas() -> ExternResult<Vec<Record>>;

#[hdk_extern]
pub fn set_active_persona(persona_hash: ActionHash) -> ExternResult<()>;

#[hdk_extern]
pub fn get_active_persona() -> ExternResult<Option<Record>>;

// Cross-applet integration
#[hdk_extern]
pub fn get_identity_for_context(context: String) -> ExternResult<Option<Record>>;
```

## 4. Address Book Coordinator Zome

```rust
// Contact functions
#[hdk_extern]
pub fn create_contact(contact: Contact) -> ExternResult<Record>;

#[hdk_extern]
pub fn update_contact(
    original_action_hash: ActionHash,
    updated_contact: Contact,
) -> ExternResult<Record>;

#[hdk_extern]
pub fn delete_contact(action_hash: ActionHash) -> ExternResult<ActionHash>;

#[hdk_extern]
pub fn get_contact(action_hash: ActionHash) -> ExternResult<Option<Record>>;

#[hdk_extern]
pub fn get_all_contacts() -> ExternResult<Vec<Record>>;

#[hdk_extern]
pub fn search_contacts(query: String) -> ExternResult<Vec<Record>>;

#[hdk_extern]
pub fn get_contacts_by_group(group: String) -> ExternResult<Vec<Record>>;

// Integration functions
#[hdk_extern]
pub fn sync_with_identity() -> ExternResult<()>;

#[hdk_extern]
pub fn get_contact_by_agent(agent: AgentPubKey) -> ExternResult<Option<Record>>;
```

## 5. Membrane Manager Coordinator Zome

```rust
// Membrane functions
#[hdk_extern]
pub fn create_membrane(membrane: Membrane) -> ExternResult<Record>;

#[hdk_extern]
pub fn update_membrane(
    original_action_hash: ActionHash,
    updated_membrane: Membrane,
) -> ExternResult<Record>;

#[hdk_extern]
pub fn delete_membrane(action_hash: ActionHash) -> ExternResult<ActionHash>;

#[hdk_extern]
pub fn get_membrane(action_hash: ActionHash) -> ExternResult<Option<Record>>;

#[hdk_extern]
pub fn get_my_membranes() -> ExternResult<Vec<Record>>;

#[hdk_extern]
pub fn get_public_membranes() -> ExternResult<Vec<Record>>;

// Member management
#[hdk_extern]
pub fn join_membrane(membrane_hash: ActionHash) -> ExternResult<()>;

#[hdk_extern]
pub fn leave_membrane(membrane_hash: ActionHash) -> ExternResult<()>;

#[hdk_extern]
pub fn invite_to_membrane(
    membrane_hash: ActionHash,
    invitee: AgentPubKey,
) -> ExternResult<()>;

#[hdk_extern]
pub fn remove_from_membrane(
    membrane_hash: ActionHash,
    member: AgentPubKey,
) -> ExternResult<()>;

#[hdk_extern]
pub fn get_membrane_members(membrane_hash: ActionHash) -> ExternResult<Vec<AgentPubKey>>;

// Integration with other applets
#[hdk_extern]
pub fn get_membranes_for_agent(agent: AgentPubKey) -> ExternResult<Vec<Record>>;
```

## 6. Communication Coordinator Zome

```rust
// Message functions
#[hdk_extern]
pub fn create_message(message: Message) -> ExternResult<Record>;

#[hdk_extern]
pub fn update_message(
    original_action_hash: ActionHash,
    updated_message: Message,
) -> ExternResult<Record>;

#[hdk_extern]
pub fn delete_message(action_hash: ActionHash) -> ExternResult<ActionHash>;

#[hdk_extern]
pub fn get_message(action_hash: ActionHash) -> ExternResult<Option<Record>>;

// Thread management
#[hdk_extern]
pub fn get_thread_messages(thread_root: ActionHash) -> ExternResult<Vec<Record>>;

#[hdk_extern]
pub fn get_replies_to_message(message_hash: ActionHash) -> ExternResult<Vec<Record>>;

// Message retrieval
#[hdk_extern]
pub fn get_direct_messages(with_agent: AgentPubKey) -> ExternResult<Vec<Record>>;

#[hdk_extern]
pub fn get_membrane_messages(membrane_hash: ActionHash) -> ExternResult<Vec<Record>>;

#[hdk_extern]
pub fn get_public_messages() -> ExternResult<Vec<Record>>;

#[hdk_extern]
pub fn get_my_messages() -> ExternResult<Vec<Record>>;

// Message status
#[hdk_extern]
pub fn mark_message_read(message_hash: ActionHash) -> ExternResult<()>;

#[hdk_extern]
pub fn react_to_message(
    message_hash: ActionHash,
    reaction: String,
) -> ExternResult<()>;

// Integration functions
#[hdk_extern]
pub fn send_to_membrane(
    membrane_hash: ActionHash,
    message: Message,
) -> ExternResult<Record>;

#[hdk_extern]
pub fn attach_file_to_message(
    message_hash: ActionHash,
    file_hash: ActionHash,
) -> ExternResult<()>;
```

## 7. Frames (Files) Coordinator Zome

```rust
// File functions
#[hdk_extern]
pub fn create_file(file: File) -> ExternResult<Record>;

#[hdk_extern]
pub fn update_file_metadata(
    original_action_hash: ActionHash,
    updated_file: File,
) -> ExternResult<Record>;

#[hdk_extern]
pub fn delete_file(action_hash: ActionHash) -> ExternResult<ActionHash>;

#[hdk_extern]
pub fn get_file(action_hash: ActionHash) -> ExternResult<Option<Record>>;

#[hdk_extern]
pub fn get_my_files() -> ExternResult<Vec<Record>>;

#[hdk_extern]
pub fn search_files(query: String) -> ExternResult<Vec<Record>>;

// Collection functions
#[hdk_extern]
pub fn create_collection(collection: Collection) -> ExternResult<Record>;

#[hdk_extern]
pub fn update_collection(
    original_action_hash: ActionHash,
    updated_collection: Collection,
) -> ExternResult<Record>;

#[hdk_extern]
pub fn delete_collection(action_hash: ActionHash) -> ExternResult<ActionHash>;

#[hdk_extern]
pub fn get_collection(action_hash: ActionHash) -> ExternResult<Option<Record>>;

#[hdk_extern]
pub fn get_my_collections() -> ExternResult<Vec<Record>>;

#[hdk_extern]
pub fn add_to_collection(
    collection_hash: ActionHash,
    item_hash: ActionHash,
) -> ExternResult<()>;

#[hdk_extern]
pub fn remove_from_collection(
    collection_hash: ActionHash,
    item_hash: ActionHash,
) -> ExternResult<()>;

#[hdk_extern]
pub fn get_collection_items(collection_hash: ActionHash) -> ExternResult<Vec<Record>>;

// Integration functions
#[hdk_extern]
pub fn get_files_by_type(file_type: String) -> ExternResult<Vec<Record>>;

#[hdk_extern]
pub fn get_media_files() -> ExternResult<Vec<Record>>;

#[hdk_extern]
pub fn get_meme_collection() -> ExternResult<Option<Record>>;

#[hdk_extern]
pub fn get_sticker_collection() -> ExternResult<Option<Record>>;
```

## 8. Dashboard Coordinator Zome

```rust
// Dashboard functions
#[hdk_extern]
pub fn create_dashboard_view(view: DashboardView) -> ExternResult<Record>;

#[hdk_extern]
pub fn update_dashboard_view(
    original_action_hash: ActionHash,
    updated_view: DashboardView,
) -> ExternResult<Record>;

#[hdk_extern]
pub fn delete_dashboard_view(action_hash: ActionHash) -> ExternResult<ActionHash>;

#[hdk_extern]
pub fn get_dashboard_view(action_hash: ActionHash) -> ExternResult<Option<Record>>;

#[hdk_extern]
pub fn get_my_dashboard_views() -> ExternResult<Vec<Record>>;

#[hdk_extern]
pub fn set_default_dashboard(view_hash: ActionHash) -> ExternResult<()>;

#[hdk_extern]
pub fn get_default_dashboard() -> ExternResult<Option<Record>>;

// Aggregation functions - integrate with other zomes
#[hdk_extern]
pub fn get_timeline_data(
    start: Timestamp,
    end: Timestamp,
) -> ExternResult<TimelineData>;

#[hdk_extern]
pub fn get_recent_messages(limit: u32) -> ExternResult<Vec<Record>>;

#[hdk_extern]
pub fn get_upcoming_events(limit: u32) -> ExternResult<Vec<Record>>;

#[hdk_extern]
pub fn get_dashboard_summary() -> ExternResult<DashboardSummary>;

// Cross-zome data aggregation
#[hdk_extern]
pub fn refresh_dashboard_data() -> ExternResult<()>;
```

## Inter-Zome Communication Patterns

```rust
// Common pattern for calling other zomes
pub fn call_other_zome<T: serde::de::DeserializeOwned>(
    zome_name: &str,
    function_name: &str,
    payload: impl serde::Serialize,
) -> ExternResult<T> {
    let response = call(
        CallTargetCell::OtherCell(CellId::from((
            DnaHash::from_raw_36(vec![0; 36]),
            agent_info()?.agent_latest_pubkey,
        ))),
        zome_name.into(),
        function_name.into(),
        None,
        payload,
    )?;

    match response {
        ZomeCallResponse::Ok(bytes) => Ok(bytes.decode()?),
        ZomeCallResponse::Unauthorized(_, _, _, _) => Err(wasm_error!(WasmErrorInner::Guest(
            "Unauthorized zome call".into()
        ))),
        ZomeCallResponse::NetworkError(_) => Err(wasm_error!(WasmErrorInner::Guest(
            "Network error in zome call".into()
        ))),
        ZomeCallResponse::CountersigningSession(_) => Err(wasm_error!(WasmErrorInner::Guest(
            "Unexpected countersigning session".into()
        ))),
    }
}
```

## Error Types

```rust
#[derive(Debug, thiserror::Error)]
pub enum MemeticMessengerError {
    #[error("Entry not found: {0}")]
    EntryNotFound(String),

    #[error("Permission denied: {0}")]
    PermissionDenied(String),

    #[error("Invalid input: {0}")]
    InvalidInput(String),

    #[error("Integration error: {0}")]
    IntegrationError(String),

    #[error("Serialization error: {0}")]
    SerializationError(String),
}
```

---

*These function signatures provide the complete API interface for implementing all 8 applets with proper inter-zome communication.*