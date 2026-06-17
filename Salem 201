from fastapi import FastAPI
from pydantic import BaseModel
from typing import List, Optional
import datetime

app = FastAPI(title="Archive App API")

# نموذج القالب / التصنيف
class Category(BaseModel):
    id: int
    name: str

# نموذج الملاحظة المطور
class Note(BaseModel):
    id: int
    category_id: int
    title: str
    content: str
    tags: List[str]
    bg_color: str
    created_at: str

categories_db = []
notes_db = []
category_counter = 1
note_counter = 1

@app.post("/api/categories")
def add_category(name: str):
    global category_counter
    new_cat = Category(id=category_counter, name=name)
    categories_db.append(new_cat)
    category_counter += 1
    return {"message": "تمت إضافة القالب بنجاح", "category": new_cat}

@app.get("/api/categories", response_model=List[Category])
def get_categories():
    return categories_db

@app.post("/api/notes")
def add_note(category_id: int, title: str, content: str, tags: str, bg_color: str = "#FFFFFF"):
    global note_counter
    tags_list = [tag.strip() for tag in tags.split(",")] if tags else []
    new_note = Note(
        id=note_counter,
        category_id=category_id,
        title=title,
        content=content,
        tags=tags_list,
        bg_color=bg_color,
        created_at=datetime.datetime.now().strftime("%Y-%m-%d %H:%M:%S")
    )
    notes_db.append(new_note)
    note_counter += 1
    return {"message": "تمت إضافة الملاحظة بنجاح", "note": new_note}

@app.get("/api/notes", response_model=List[Note])
def get_notes(search_query: Optional[str] = None):
    if search_query:
        result = [n for n in notes_db if search_query in n.title or search_query in n.content or search_query in n.tags]
        return result
    return notes_db
