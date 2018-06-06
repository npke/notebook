# SQLAlchemy - ORM

## Connecting
- to connect use `create_engine` from `sqlalchemy` 
- pass the keyword argument `echo=True` to enable logging
- `create_engine` return an Engine instance represents the core interface to the database
- the typical form of database url is `dialect+driver://username:password@host:port/database`
- popular dialect for MySQL is `PyMySQL`
```python
from sqlalchemy import create_engine
engine = create_engnine(database_url)

# PostgreSQL 
# engine = create_engine('postgresql://scott:tiger@localhost/mydatabase')

# MySQL
# engine = create_engine('mysql+pymysql://scott:tiger@localhost/foo')
```
- the first time methods like `Engine.execute()` or `Enginge.connect()` then the connection is established


## Mapping
- declarative base class maintains a catalog of classes and tables relative to that base, created by using `declarative_base()`
```python
Base = declarative_base()
```
- other model classes are defined with details about table like name, columns, data types
```python
from sqlalchemy import String, Integer, Column

class User(Base):
    __tablename__ = 'users'

    id = Column(Integer, primary_key=True)
    name = Column(String)
    email = Column(String)
    password = Column(String)

    def __repr__(self):
        return "<User(id='%s', name='%s', email='%s', passwod='%s')>" % (self.id, self.name, self.email, self.password)
```
- can use mixin to define common table options, columns across classes
- when class is constructed all `Column` objects is replaced with [descriptors](https://docs.sqlalchemy.org/en/latest/glossary.html#term-descriptors) (this process is called [instrumentation](https://docs.sqlalchemy.org/en/latest/glossary.html#term-instrumentation))
- the `Table` object then is associated with the class by mapping a `Mapper` object
- the `Table` object is member of a larger collection known as `MetaData` available in the `.metadata` of declarative base class.
- `MetaData` has the ability to emit a limited set of schema generation commands to the database.
- use `MetaData.create_all(engine)` to create tables that don't exist yet
```python
Base.metadata.create_all(engine)
```

## Model instances
- pass keyword arguments to create an instance
```python
user = User(name='Ke Nguyen', email='ke.nguyen@gmail.com', password='whyneedpassword')
```

## Session
- the ORM's handle to the database is the `Session`
- when need to interact with the database instantiate a session `Session()`
```python
from sqlalchemy.orm import sessionmaker
Session = sessionmaker(bind=engine)

session = Session()
```
- the session is associated with the engine but hasn't open any connection
- when it's first used, it retreive a connection from connection pool then hold on it until all changes are committed or the session object is closed.

## CRUD
### Create
- to persit data use `Session.add(object)`
- the operations can be `pending` and then `flush` to the database as soon as needed.
- bulk create using `Session.add_all([object])`
- issue remaining change by `Session.commit()` then the objects is refreshed with created data (like auto increment id)
- rollback the operations by `Session.rollback()`

### Read
- use `Session.query(Class)` to instantiate a `Query` object
- combine with `filter_by`, `filter`, `order_by` to filter the result
```python
session.query(User).filter(User.name.in_(['ed', 'fakeuser'])).all()
# => [<User(name='ed', fullname='Ed Jones', password='f8s7ccs')>]

for instance in session.query(User).order_by(User.id):
    print(instance.name, instance.fullname)

for name, fullname in session.query(User.name, User.fullname):
    print(name, fullname)
```
- the returned result can be list or tuple
- control the names of individual column expressions using the `label()`
```python
for row in session.query(User.name.label('name_label')).all():
    print(row.name_label)

```
- LIMIT, OFFSET is conveniently using Python array slices and typically in conjunction with ORDER BY
```python
for u in session.query(User).order_by(User.id)[1:3]:
    print(u)
```
- [common filter operators](https://docs.sqlalchemy.org/en/latest/orm/tutorial.html#common-filter-operators)
- counting use `count()` method or `func.count()`

### Delete
- use the `Session.delete()` method
```python
session.delete(user)
```
- cascade delete need to be configured
```python
class User(Base):
    __tablename__ = 'users'

    id = Column(Integer, primary_key=True)
    name = Column(String)
    fullname = Column(String)
    password = Column(String)

    addresses = relationship("Address", back_populates='user', cascade="all, delete, delete-orphan")
    def __repr__(self):
    return "<User(name='%s', fullname='%s', password='%s')>" % (
                            self.name, self.fullname, self.password)

```
### Update
> pass

## Relationships
- use `ForeignKey` and `relationship` method to create relationships between classes
```python
from sqlalchemy import ForeignKey
from sqlalchemy.orm relationship

class Address(Base):
    __tablename__ = 'addresses'
    id = Column(Integer, primary_key=True)
    user_id = Column(Integer, ForeignKey('users.id'))
    name = Column(String(255), nullable=False)
    user = relationship('User', back_populates='addresses')

    def __repr__(self):
        return "<Address(id='%s', name='%s')>" % (self.id, self.name)

User.addresses = relationship('Address', order_by=Address.id, back_populates='user')
```
- related object can be add directly to the instance once the changes are committed new data is persisted to the database
```python
jack = User(name='jack', fullname='Jack Bean', password='gjffdd')
jack.addresses # => []

jack.addresses = [
    Address(email_address='jack@google.com'),
    Address(email_address='j25@yahoo.com')]

jack.addresses[1] # => <Address(email_address='j25@yahoo.com')>

jack.addresses[1].user # => <User(name='jack', fullname='Jack Bean', password='gjffdd')>

session.add(jack)
session.commit()

jack = session.query(User).filter_by(name='jack').one()
jack # => <User(name='jack', fullname='Jack Bean', password='gjffdd')>

jack.addresses # => [<Address(email_address='jack@google.com')>, <Address(email_address='j25@yahoo.com')>]
```

## Query with joins
- user `Query.join()` method
```python
query.join(Address, User.id==Address.user_id)    # explicit condition
query.join(User.addresses)                       # specify relationship from left to right
query.join(Address, User.addresses)              # same, with explicit target
query.join('addresses')                          # same, using a string
```