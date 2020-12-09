# cims-documentation

An application for managing (Classroom) Documents like Notes, Syllabus, Notices e.t.c. These documents can be oraganised by labels in separate rooms. These same labels can then later be used to filter these documents so as to make finding documents easier. 

Although we initially set out to create this only for classroom documents as stated above, the app can very well be used for other similar purposes where documents are needed to be organised and shared in some group.

There will also be a task list in each room which can be used to track progress, like syllabus for classrooms. 

## Architecture

![Architecture Diagram](/ArchitectureDiagram.png)


# [Room Service](https://github.com/tabishmahfuz1/room-service)
```
scalar Date

type Query {
    rooms(inp: RoomFilterInput): [Room]!
    posts(inp: PostFilterInput!): [Post]!
}

type Mutation {
    createRoom(inp: CreateRoomInput!): Room!
    updateRoom(inp: UpdateRoomInput!): Room!
    saveTasks(roomId: ID!, task: [TaskItemInput!]!): [Task!]!
    addMember(roomId: ID!, member: String!): Room!
    removeMember(roomId: ID!, member: String!): Room!
    joinRoom(roomCode: String!): Room!
    createPost(post: PostInput!): Post!
}

type Room {
    id: ID!
    name: String!
    code: String!
    members: [String!]!
    posts: [Post]!
    tasks: [Task]
    createdBy: String!
    createdAt: Date!
}

type Task {
    id: String!
    name: String!
    customAttrs: String
    room: Room# Maybe not needed?
}

input TaskInput {
    id: String
    name: String!
    customAttrs: String
}

type Post {
    id: ID!
    type: PostType!
    room: Room!
    attachments: [File]!
    text: String
    labels: [String!]!
    parentPost: Post
    childPosts: [Post]
    createdBy: String!
    createdAt: Date!
}

type File {
    id: ID!
    name: String!
    file: String!
}

input CreateRoomInput {
    name: String!
    members: [String!]
}

input UpdateRoomInput {
    id: ID!
    name: String
}


input RoomFilterInput {
    id: ID
    name: String
    code: String
    member: [String]
    createdBy: String
}

input PostFilterInput {
    id: ID
    type: PostType
    text: String
    room: String
    parentPost: ID
    createdBy: String
}

input PostInput {
    room: ID!
    type: PostType!
    attachments: [Upload]!
    text: String
    labels: [String!]!
    parentPost: ID
}
```

# [File Storage Service](https://github.com/tabishmahfuz1/file-storage-service)
## POST: file-upload
**ContentType:** multipart-formdata

**Params:**

    - file: mutipart File
    
    - name?: string
    
    - directory?: string
    
    - labels: JSON Array (String)
    
    - mimetype: String

## POST: search
**ContentType:** application/json

**Params:**

    - labels: Array<String>

## GET: /get/:id
**Params:** *id* **NOTE**: This is a URL parameter

**Description:** *Returns the file specified by the id*

# [Document Parser](https://github.com/tabishmahfuz1/document-parser)
## POST: /
**ContentType:** multipart-formdata
**Description:** *Returns Labels for a document*
**Prams:**
    - file: multipart file

# [CIMS Frontend](https://github.com/tabishmahfuz1/cims-frontend)
**Description:** *React based frontend for CIMS*
