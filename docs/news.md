### Sequence Diagram
```mermaid
sequenceDiagram
    participant User as User
    participant UI as UI
    participant Backend as Backend
    participant TempDB as Temp Data Store
    participant JeeniDB as Jeeni Database
    participant Mathpix as Mathpix
    participant GenAI as GenAI Processor

    User ->> UI: Select Course, Subject, Chapter, Get Questions
    UI ->> Backend: getAllCourses, getSubjectFromCourse, getChapterFromSubject, 
           getQuestionsForChapter/getQuestionsForQuePaper  


    UI ->> Backend: Retrieve Questions & Images
    Backend ->> JeeniDB: `GET /chapters/{chapter_id}/questions` or `GET /question-paper/{paper_id}/questions`
    Backend ->> TempDB: `POST /conversion/temp-list` (storeDataForProcessing)

    loop Process Each Question
        Backend ->> TempDB: `GET /conversion/temp-list/{temp_list_id}` (fetchNextQuestion)
        Backend ->> GenAI: `POST /conversion/temp-list/{temp_list_id}/process` (createTempListForConversion)
        GenAI -->> Backend: Return Processed Data
        Backend ->> TempDB: `PUT /conversion/temp-list/{temp_list_id}` (updateTempListForConversion)
    end

    Backend ->> Mathpix: `PUT /mathpix/update-questions`
    Backend ->> TempDB: `PUT /conversion/temp-list/{temp_list_id}/complete`
    Backend ->> JeeniDB: `PUT /jeenidb/join-table`
    Backend ->> TempDB: `DELETE /conversion/temp-list/{temp_list_id}` (deleteTempListForConversion)

    UI ->> TempDB: `GET /conversion/temp-list/{temp_list_id}/progress`
    Backend ->> JeeniDB: `GET /jeenidb/progress` (processTempList)
    Backend ->> Mathpix: `GET /mathpix/progress`
    TempDB ->> UI: Return Processing Status
    UI -->> User: Display Progress (getDataFromTempList)
```
