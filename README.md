# 2024fastAPI
fastAPI를 이용한 todolist 만들기, 2024 1학기 캡스톤프로젝트


<div align=center> 
  <img src="https://img.shields.io/badge/python-3776AB?style=for-the-badge&logo=python&logoColor=white"> 
  <br>
    <img src=https://img.shields.io/badge/FastAPI-005571?style=for-the-badge&logo=fastapi>
  <img src=https://img.shields.io/badge/fastify-%23000000.svg?style=for-the-badge&logo=fastify&logoColor=white>

  <br>
  
  <img src="https://img.shields.io/badge/html5-E34F26?style=for-the-badge&logo=html5&logoColor=white"> 
  <img src="https://img.shields.io/badge/css-1572B6?style=for-the-badge&logo=css3&logoColor=white"> 
  <img src="https://img.shields.io/badge/javascript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black"> 


  <br>


<img src="https://img.shields.io/badge/github-181717?style=for-the-badge&logo=github&logoColor=white">
</div>


## ToDo리스트 만들기


from fastapi import FastAPI, HTTPException
from pydantic import BaseModel
from typing import List

app = FastAPI()

# 간단한 할 일 데이터 모델
class TodoItem(BaseModel):
    id: int
    title: str
    description: str = None

# 임시 데이터베이스용 리스트
todo_db = []

# 할 일 추가
@app.post("/todos/", response_model=TodoItem)
async def create_todo_item(item: TodoItem):
    todo_db.append(item)
    return item

# 모든 할 일 목록 가져오기
@app.get("/todos/", response_model=List[TodoItem])
async def get_todo_items():
    return todo_db

# 특정 ID의 할 일 가져오기
@app.get("/todos/{item_id}", response_model=TodoItem)
async def get_todo_item(item_id: int):
    for item in todo_db:
        if item.id == item_id:
            return item
    raise HTTPException(status_code=404, detail="Item not found")

# 특정 ID의 할 일 삭제하기
@app.delete("/todos/{item_id}")
async def delete_todo_item(item_id: int):
    for index, item in enumerate(todo_db):
        if item.id == item_id:
            del todo_db[index]
            return {"message": "Item deleted successfully"}
    raise HTTPException(status_code=404, detail="Item not found")
