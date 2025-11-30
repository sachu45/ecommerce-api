from fastapi import FastAPI, HTTPException
from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker, Session
from pydantic import BaseModel
import uvicorn

app = FastAPI(title="E-Commerce API", version="1.0")

# Database Setup
DATABASE_URL = "mysql+pymysql://user:password@localhost/ecommerce"
engine = create_engine(DATABASE_URL)
SessionLocal = sessionmaker(bind=engine)

# Models
class Product(BaseModel):
    id: int
    name: str
    price: float
    stock: int

@app.post("/products/")
def create_product(product: Product, db: Session = SessionLocal()):
    # Add to database
    return {"message": "Product created", "product": product}

@app.get("/products/{product_id}")
def get_product(product_id: int):
    return {"id": product_id, "name": "Sample Product", "price": 999}

if __name__ == "__main__":
    uvicorn.run(app, host="0.0.0.0", port=8000)

