# springboot-reply-Projections.bean
study

```mermaid
flowchart TD
    %% Nodes
    Client["Client <br/> (Swagger / JS)"]
    Controller["<b>UpDownController</b> <br/> upload(UploadFileDTO)"]
    CheckFile{"if <br/> uploadFileDTO.getFiles() <br/> != null"}
    Loop["<b>forEach Loop</b> <br/> (multipartFile)"]
    
    subgraph "File Process Logic"
        UuidGen["UUID 생성 <br/> UUID.randomUUID()"]
        PathGen["저장 경로 생성 <br/> C:\\upload\\uuid_originalName"]
        SaveFile["<b>파일 저장</b> <br/> multipartFile.transferTo(savePath)"]
        
        CheckImg{"if <br/> ContentType startsWith <br/> 'image'"}
        ThumbGen["<b>썸네일 생성</b> <br/> Thumbnailator.createThumbnail <br/> (200x200)"]
        SetImgFlag["image = true"]
    end
    
    DTO["<b>UploadResultDTO 생성</b> <br/> builder().uuid().fileName().img().build()"]
    ListAdd["list.add(dto)"]
    Return["<b>Return</b> <br/> List&lt;UploadResultDTO&gt;"]

    %% Flow
    Client -->|POST /upload <br/> multipart/form-data| Controller
    Controller --> CheckFile
    CheckFile -- true --> Loop
    CheckFile -- false --> Return
    
    Loop --> UuidGen
    UuidGen --> PathGen
    PathGen --> SaveFile
    SaveFile --> CheckImg
    
    CheckImg -- true --> ThumbGen
    ThumbGen --> SetImgFlag
    SetImgFlag --> DTO
    
    CheckImg -- false --> DTO
    
    DTO --> ListAdd
    ListAdd --> Loop
    
    Loop -- Loop End --> Return
    Return -->|"JSON Array"| Client

    %% Styling
    style Controller fill:#e1f5fe,stroke:#01579b,stroke-width:2px
    style SaveFile fill:#ffccbc,stroke:#bf360c
    style ThumbGen fill:#ffe082,stroke:#ff6f00
    style Return fill:#c8e6c9,stroke:#2e7d32
    style Controller fill:#e1f5fe,stroke:#01579b,stroke-width:2px
    style SaveFile fill:#ffccbc,stroke:#bf360c
    style ThumbGen fill:#ffe082,stroke:#ff6f00
    style Return fill:#c8e6c9,stroke:#2e7d32```
