# wos-protocol-spec README

## The World Object Store (WOS)

The WOS is a object store for real world and physical world objects built on a decentralized backbone.  Objects can be anything, including audio clips, images, 3d objects, pins, animations, essentially anything that can exist in a world.  WOS stores ARCs (Augmented Reality Creations), allows pinning them to the physical world, stores them in IPFS, creates S3 CDN, indexes them, and renders thumbnails for easy consumption on different devices, creates ERC-721 smart contracts and mints NFTs referencing the objects.  

ARCs are stored in the World Object Store (WOS) by creators and consumed by users.  Being decentralized, any creator or user can interact with the WOS via mobile apps or by any dapp that speaks the WOS protocol.  Each ARC is stored in a WOS bucket located at an IPFS hash address.

The WOS is a layered technology including decentralized IPFS object store, encryptor, index, protocol/API, and background services.  The bottom layer is the decentralized store.  The encryptor layer manages encryption, decryption, and signing of ARCs in the store.  Encryption is required for private ARCs, private being restricted to limited groups of people.  Signing is required for time-limited access to ARCs. The index catalogs all the ARCs in the WOS for fast search.  Without the index, search would be brute-force and slow.  WOS addresses are meaningless and require the index for search.  Thumbnail rendering is important because of inconsistencies between formats used between Android and Apple.  The top-most layer is the CDN.  This layer is responsible for distributing ARCs across the globe for fast access.

## WOS Protocol

The WOS Protocol defines how objects are structured and the API.  The object format is a declarative JSON-based scheme allowing for virtually any kind of object representation, including images, mpeg files, 3D-objects, pins, and code. v1 WOS API is a centralized REST API sitting on top of S3, encryptors, and IPFS.  See [arc-spec](https://github.com/wos-project/arc-spec) for details on ARCs.

An object stored in the WOS is accessible both through the WOS and through IPFS using a content identifier.  The general http format of a WOS object is https://worldos.earth/object/<addr> where the address is a IPFS content identfier.  The index.json file is returned when using the GET method.  Depending on what kind of object it is, an arc, pin, or pinned arc is returned.  Each object knows its kind.

WOS objects exist in IPFS and are owned by a cryptographic wallet holder.  Ownership is not represented in the object, but is represented in an NFT referencing the object. 

API
Base address is https://worldos.earth/api/v1 

Use https://worldos.earth/object/<addr> or https://ipfs.io/ipfs/<addr> to access an object


| Method | Path | Notes | 
| ------ | ---- | ----- |
| POST | /object/archive/multipart | Upload object tar file |
| GET | /object/:addr/archive | Get tar object archive |
| POST | /object/index | Upload object index |
| GET  | /object/:addr/index | Get index |
| GET |  /object/search | Search arcs, pins, pinned arcs |
| POST | /object/batchUpload | Start batch upload object |
| POST | /object/batchUpload/multipart/:sessionId | Multipart upload a file |
| PUT | /object/batchUpload/:sessionId | End batch upload |
| GET | /layers | List layers |

---

### POST /object/archive/multipart

Creates object from tar archive

Parameters: none
Body: multipart form tar file
Response: addr

---

### GET /object/:addr/archive

Gets object archive at address

Parameters: address of object
Body: none
Response: object archive tar file

---

### POST /object/index

Creates an object as a single index file

Parameters: none
Body: index JSON
Response: address

---

### GET /object/:addr/index

Gets an object index JSON

Parameters: address of object
Body: none
Response: address

---

### GET /search

Search for arcs, pins, pinned arcs

Parameters: query string search arguments
Body: none
Response: JSON list of names, description, thumbs, labels

---

### POST /object/batchUpload

Starts batch upload and returns session ID

Parameters: none
Body: none
Response: sessionId

---

### POST /object/batchUpload/multipart/:sessionId

Uploads a file given batch upload sessionId

Parameters: sessionId
Body: multipart form upload
Response: status

---

### PUT /object/batchUpload/:sessionId

Finalizes batch upload process and begins processing of the object

Parameters: sessionId
Body: none
Response: status

---

### GET /layers

Get list of layers

Parameters: none
Body: none
Response: JSON list of layers

