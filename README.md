## Flask-Migrate tutorial

- モデル定義とマイグレーションの有効化
```python
app = Flask(__name__)
app.config['SQLALCHEMY_DATABASE_URI'] = f"postgresql://{os.getenv('DB_USER')}:{os.getenv('DB_PASSWORD')}@{os.getenv('DB_HOST')}:{os.getenv('DB_PORT')}/{os.getenv('DB_NAME')}"
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False
db = SQLAlchemy(app)
migrate = Migrate(app,db)

class Task(db.Model):
    __tablename__ = 'task'
    id = db.Column(db.Integer, primary_key=True)
    content = db.Column(db.String(200), nullable=False)
    completed = db.Column(db.Boolean, default=False)
```

- 初回のみ
```
flask db init
```

- dbのマイグレート
```
flask db migrate
```

- dbへ反映
```
flask db upgrade
```

- 確認
```
→ docker compose exec -it db bash
root@381790e6334c:/# psql -U admin -d todo

psql (16.6 (Debian 16.6-1.pgdg120+1))
Type "help" for help.
todo=# \d task
                                     Table "public.task"
  Column   |          Type          | Collation | Nullable |             Default              
-----------+------------------------+-----------+----------+----------------------------------
 id        | integer                |           | not null | nextval('task_id_seq'::regclass)
 content   | character varying(200) |           | not null | 
 completed | boolean                |           |          | 
Indexes:
    "task_pkey" PRIMARY KEY, btree (id)

```