### [Defining Entities](https://mikro-orm.io/docs/defining-entities)
- Entities are POJO
- Decorated class has `Entity` with properties with another decorator
- Including `{ wrappedEntity: true }` in your `Ref` property definitions will wrap the reference, providing access to helper methods like `.load` and `.unwrap`, which can be helpful for loading data and changing the type of your references where you plan to use them.
```ts
	@Entity()
	export class Book extends CustomBaseEntity {
	
	  @Property()
	  title!: string;
	
	  @ManyToOne(() => Author)
	  author!: Author;
	
	  @ManyToOne(() => Publisher, { ref: true, nullable: true })
	  publisher?: Ref<Publisher>;
	
	  @ManyToMany({ entity: 'BookTag', fixedOrder: true })
	  tags = new Collection<BookTag>(this);
	
	}
```
-  Optional Properties `{ nullable: true }`
- Default Values
```ts
	@Property({ default: 1 })  
	foo!: number & Opt;
	//foo: number & Opt = 1;
```
- Enums `@Enum(() => UserRole) role!: UserRole;`
	- [ ] Postgres Native Enum 
	`@Enum({ items: () => UserRole, nativeEnumName: 'user_role' })`
	-  Enum Array
	`@Enum({ items: () => Role, array: true, default: [Role.User] })`
- Map to Primary Keys
```ts
	@ManyToOne(() => User, { mapToPk: true })
	// user: number;
	user: [string, string]; // [first_name, last_name] composite
```
- [ ] Formula - SQL snippet to entity
```ts
	@Formula(alias => `${alias}.obj_length * ${alias}.obj_height * ${alias}.obj_width`)
	// @Formula('obj_length * obj_height * obj_width') - could throw NonUniqueFieldNameException
	objectVolume?: number;
```
- [ ] Index
- [ ] Unique
```ts
	@Entity()
	@Index({ properties: ['name', 'age'] }) 
	// compound index, with generated name
	@Index({ name: 'custom_idx_name', properties: ['name'] }) 
	// simple index, with custom name
	@Unique({ properties: ['name', 'email'] })
	export class Author {
	
	  @Property()
	  @Unique()
	  email!: string;
	
	  @Property()
	  @Index() // generated name
	  age?: number;
	
	  @Index({ name: 'born_index' })
	  @Property()
	  born?: Date;
	
	  @Index({ name: 'custom_index_expr', expression: 'alter table `author` add index `custom_index_expr`(`title`)' })
	  @Property()
	  title!: string;
	
	}
```

- [ ]  Check
```ts
	@Entity()
	// with generated name based on the table name
	@Check({ expression: 'price1 >= 0' })
	// with explicit name
	@Check({ name: 'foo', expression: columns => `${columns.price1} >= 0` })
	// with explicit type argument we get autocomplete on `columns`
	@Check<FooEntity>({ expression: columns => `${columns.price1} >= 0` })
	export class Book {
	
	  @PrimaryKey()
	  id!: number;
	
	  @Property()
	  price1!: number;
	
	  @Property()
	  @Check({ expression: 'price2 >= 0' })
	  price2!: number;
	
	  @Property({ check: columns => `${columns.price3} >= 0` })
	  price3!: number;
	
	}
```

- Lazy (`@Property({ lazy: true })`) - omit from select
	- `em.find(Book, 1, { populate: ['text'] });`  - to load the field
	- if already loaded, we need to pass `refresh: true`
- [ ]  `ScalarReference` wrapper
- Virtual Properties - method or get/set
```ts
	@Entity()
	export class User {
	
	  [HiddenProps]?: 'firstName' | 'lastName';
	
	  @Property({ hidden: true })
	  firstName!: string;
	
	  @Property({ hidden: true })
	  lastName!: string;
	
	  @Property({ name: 'fullName' })
	  getFullName() {
	    return `${this.firstName} ${this.lastName}`;
	  }
	
	  @Property({ persist: false })
	  // otherwise gets saved into database
	  get fullName2() {
	    return `${this.firstName} ${this.lastName}`;
	  }
	}
	// console.log(wrap(author).toJSON()); 
	// { fullName: 'Jon Snow', fullName2: 'Jon Snow' }
```
 - [ ] base entity
```ts
	import { v4 } from 'uuid';
	
	// @Entity({ abstract: true })
	// required if we are using folder based discovery and it does not have any property decorator
	export abstract class CustomBaseEntity {
	
	  @PrimaryKey()
	  uuid = v4();
	
	  @Property()
	  createdAt = new Date();
	
	  @Property({ onUpdate: () => new Date() })
	  updatedAt = new Date();
	
	}
```
- [ ] Generated SQL columns
```ts
	@Entity()
	export class User {
	
	  @PrimaryKey()
	  // @PrimaryKey({ generated: 'identity' }) - postgresql
	  // @PrimaryKey({ type: 'uuid', defaultRaw: 'gen_random_uuid()' })
	  id!: number;
	
	  @Property({ length: 50 })
	  firstName!: string;
	
	  @Property({ length: 50 })
	  lastName!: string;
	
	  @Property<User>({ length: 100, generated: cols => `(concat(${cols.firstName}, ' ', ${cols.lastName})) stored` })
	  fullName!: string & Opt;
	
	  @Property({ columnType: `varchar(100) generated always as (concat(first_name, ' ', last_name)) virtual` })
	  fullName2!: string & Opt;
	
	}
```

### [Relationships](https://mikro-orm.io/docs/relationships)
- Unidirectional - defined at owning side
		- owning side (where reference are stored) = `inversedBy`
		- inverse side = `mappedBy`
- To avoid `ReferenceError: Cannot access 'Class' before initialization` error by ESM use `Rel` (eg:`author!: Rel<Author>;`)
- `ManyToOne` - Many from current refer to one of reffered
```ts
	@Entity()
	export class Book {
	  @ManyToOne() // plain decorator, type via reflection!
	  // @ManyToOne(() => Author) // specify type manually as a callback
	  // @ManyToOne('Author') // as a string
	  // @ManyToOne({ entity: () => Author }) // or use options object
	  author!: Author;
	}
```
-  `OneToMany` - One of the current  has Many  references to the referred
```ts
	@Entity()
	export class Author {
	
	  @OneToMany(() => Book, book => book.author)
	  // @OneToMany('Book', 'author')
	  // @OneToMany({ mappedBy: book => book.author }) // referenced entity type can be sniffer too
	  // @OneToMany({ entity: () => Book, mappedBy: 'author', orphanRemoval: true })
	  books = new Collection<Book>(this);
	}
```
- `OneToOne` - foreign key column is also unique
```ts
// owning side
	@Entity()
	export class User {
	  // `owner/inverseBy/mappedBy` is not provided => owning side
	  @OneToOne()
	  // `inversedBy`=> owning one, inverse side uses `mappedBy`
	  // @OneToOne({ inversedBy: 'bestFriend' })
	
	  // specifically mark the owning side with `owner: true`
	  //@OneToOne(() => User, user => user.bestFriend, { owner: true })
	  bestFriend!: User;
	}
// inverse side
	@Entity()
	export class User {
	
	  @OneToOne({ mappedBy: 'bestFriend', orphanRemoval: true })
	  // @OneToOne(() => User, user => user.bestFriend, { orphanRemoval: true })
	  bestFriend!: User;
	
	}
```
- `ManyToMany`
```ts
// owning side
	@Entity()
	export class Book {
	  // `owner/inverseBy/mappedBy` isnot provided => owning side
	  @ManyToMany()
	  @ManyToMany(() => BookTag, 'books', { owner: true })
	  @ManyToMany(() => Author) // uni-directional many to many
	  friends: Collection<Author> = new Collection<Author>(this);
	}
// inverse side
	@Entity()
	export class BookTag {
	  // points to the owning side via `mappedBy`
	  @ManyToMany(() => Book, book => book.tags)
	  books = new Collection<Book>(this);
	}
```

### [Entity Manager](https://mikro-orm.io/docs/entity-manager)
######  `em.persist(entity)` 
- mark new entities for future persisting.
	- Makes entities managed by entity manager
		- Fetched by database (already managed)
		- By entity manager (Registered by `persist`)
	- Also persists reference entities (if book has author, persisting book will persist author as well)
	- Takes array/single element (could be chained with `flush`)
###### `em.flush()` 
- go through all managed entities
- compute appropriate change sets
- perform according database queries
- Modes (check ways to setup [here](https://mikro-orm.io/docs/unit-of-work#flush-modes))
	- - `FlushMode.COMMIT` -  delays the flush until the current Transaction is committed.
	- `FlushMode.AUTO` - (default) flushes only if necessary.
			- If I have persisted change in `MyEntity` (eg new entry), query (at same Enitity) will auto flush
	- `FlushMode.ALWAYS` - flush before every query.
- [ ] [update could happen without loading ](https://mikro-orm.io/docs/entity-manager#updating-references-not-loaded-entities)(via unit of work)
###### `em.fork()` 
- create clean entity manager (separate context) and identity map
- Usually provides request specific entity manager
###### Remove
```ts
	const book1 = em.getReference(Book, 1);
	// initialized not required, reference is enough
	await em.remove(book1).flush();

	// em.nativeDelete(); => fire simple delete query
```
###### Entity ManagerYYY
- `await em.findOne(EntityName, id);`
	- return `null` if not found
	- `em.findOneOrFail()` throws an error
- `await em.find(EntityName, {});` same as `findAll`
- `await em.upsert(Author, { email: 'foo@bar.com', age: 33 });`
-  `em.refresh(entity)` - sync changes with database (local changes lost)
	- equivalent to `findOne` with `refresh: true` and disabled auto-flush
```ts
	const books = await em.findAll(Book, {
	  where: { publisher: { $ne: null } },
	  populate: ['author.friends'],
	});

// alternatively using helper function
	const authors = await  
			em.createQueryBuilder(Author).select('*').getResult();  
	await em.populate(authors, { populate: ['books.tags'] });

// fail handler
	try {
	  const author = await em.findOneOrFail(Author, { name: 'does-not-exist' }, {
	    failHandler: (entityName: string, where: Record<string, any> | IPrimaryKey) => new Error(`Failed: ${entityName} in ${util.inspect(where)}`)
	  });
	} catch (e) {
	  console.error(e); // our custom error
	}


```
- condition/where object
```ts
// search by entity properties
	const users = await em.find(User, { firstName: 'John' });

// for searching by reference => primary key directly
	const id = 1;
	const users = await em.find(User, { organization: id });

// unpopulated reference (including `Reference` wrapper)
	const ref = await em.getReference(Organization, id);
	const users = await em.find(User, { organization: ref });

// fully populated entities 
	const ent = await em.findOne(Organization, id);
	const users = await em.find(User, { organization: ent });

// complex queries with operators
	const users = await em.find(User, {
		$and: [
			{ id: { $nin: [3, 4] } },
			{ id: { $gt: 2 } }
		] 
	});

// search for array of primary keys directly
	const users = await em.find(User, [1, 2, 3, 4, 5]);

// search by single primary key
	const user1 = await em.findOne(User, 1);
```
- using filter object/DTO
```ts
	import { PlainObject } from '@mikro-orm/core';
	
	class Filter extends PlainObject {
	  name: string;
	}
	
	const where = new Filter();
	where.name = 'Jon';
	const res = await em.find(Author, where);
```
- hacks (basically provide type explicitly)
```ts
	const books = await em.find<Book>(Book, { ... query ... });
	// await em.getRepository(Book).find({ ... query ... });
	// await em.find<any>(Book, { ... query ... }) as Book[];
```
- search by referenced fields
```ts
	// find author of a book that has tag specified by name
	const author = await em.findOne(Author, {
					books: { tags: { name: 'Tag name' } } 
				});
	console.log(author.books.isInitialized()); 
	// false, only works for query and sort
	
	const author = await em.findOne(Author, { 
						books: { tags: { name: 'Tag name' } } 
					}, { populate: ['books.tags'] }
				);
	author.books.isInitialized();//true, because populated
	author.books[0].tags.isInitialized(); // true
	author.books[0].tags[0].isInitialized(); // true
```
- [ ] `upsert`
###### Entity Reference
- `Mikro` represent all entity as object (basically class instance)
- Doesn't have to be fully loaded
- Does not have to query database
- Stored in `identity map` 
```ts
	const userRef = em.getReference(User, 1);  
	console.log(userRef); //(User){ id: 1 } => not initialized state
// actions
	// 01. setting relation properties
	author.favouriteBook = em.getReference(Book, 1);
	// 02. removing entity by reference
	em.remove(em.getReference(Book, 2));
	// 03. adding entity to collection by reference
	author.books.add(em.getReference(Book, 3));
```
###### Entity State
- `MikroORM.init()` does entity discovery
	-  patch entity prototype
	- generate lazy getter for `WrappedEntity` (holds various metadata and state information about the entity) to allow type safe access.
	- defaults `updateNestedEntities`, `updateByPrimaryKey`,   `!mergeObjectProperties`,  
`mergeEmbeddedProperties`
```ts
	import { wrap } from '@mikro-orm/core';
	
	const userRef = em.getReference(User, 1);
	console.log('userRef is initialized:', wrap(userRef)
										.isInitialized()); // false
	
	await wrap(userRef).init();
	console.log('userRef is initialized:', wrap(userRef)
										.isInitialized()); // true
```
- Extending `BaseEntity` provides `wrap` methods in class
- `Unit of work` uses the state information to compute difference
- used for serialization
```ts
// partial loading (works for nested cols, PK auto selected)
	// FK needs to be explicitly mentioned (else broken link)
	const author = await em.findOne(Author, '...', {  
		fields: ['name', 'books.title'],
	});

// exclude fields
	const author = await em.findOne(Author, '...', {
	  exclude: ['email', 'books.price'],
	  populate: ['books'], // explicitly populate unlike `field`
	});
	
// pagination
	const [authors, count] = await em.findAndCount(Author, 
						   { ... }, { limit: 10, offset: 50 }
					   ); // count is total count

// pagination- cursor based
	/* cursor object
	 Cursor<User> {  
			items: [  
			User { ... },  
			User { ... },  
			User { ... },  
			...  
			],  
			totalCount: 50,  
			length: 10,  
			startCursor: 'WzRd',  
			endCursor: 'WzZd',  
			hasPrevPage: true,  
			hasNextPage: true,  
	 } */
	const currentCursor = await em.findByCursor(User, {}, {  
		first: 10,
		// supported: before, first,(forward pagination) 
		// after(cursor), last(number) (backward)
		// unsupported: limit, offset
		after: previousCursor, // cursor instance
		// after: currentCursor.endCursor, // opaque string
		// after: { id: lastSeenId }, // entity-like POJO
		orderBy: { id: 'desc' },  // required
	});
	
// 
```
- [ ] custom SQL
```ts
	const users = await em.find(User, {
			[sql`lower(email)`]: 'foo@bar.baz' 
		}, {
		  orderBy: { 
			  [sql`(point(loc_latitude, loc_longitude) <@> point(0, 0))`]: 'ASC' 
		},
	});
	// query
	/*
	select `e0`.*
	from `user` as `e0`
	where lower(email) = 'foo@bar.baz'
	order by (point(loc_latitude, loc_longitude) <@> point(0, 0)) asc
	*/
```
- [ ] atomic updates via raw helper
- [ ] batch `insert`, `update`, `delete`
- `disableIdentityMap` (impacts performance)
	- creates new context (so `em.flush` has no effect)
	- load entities inside that (and clear afterwards)
- Custom ordering
```ts
	enum Priority {
	  Low = 'low',
	  Medium = 'medium',
	  High = 'high',
	}
	
	@Entity()
	class Task {
	  @PrimaryKey()
	  id!: number
	
	  @Property()
	  label!: string
	
	  @Enum({
	    items: () => Priority,
	    customOrder: [Priority.Low, Priority.Medium, Priority.High]
	  })
	  priority!: Priority
	}
```

### [Entity Helper](https://mikro-orm.io/docs/entity-helper)
- `assign()` 
	- does not merge recursively (like `Object.assign`)
```ts
	import { wrap } from '@mikro-orm/core';
	// const jon = new Author('Jon Snow', 'snow@wall.st');
	// const book = new Book('Book', jon);
	// book.author = em.getReference(Author, '...id...');
	wrap(book).assign(
		{
		  title: 'Better Book 1',
		  author: '...id...',
		},
		// { 
		//	em  // for not managed entities
		//	mergeObjectProperties: true  -- if false, replace object
	//- for object { bar: { foo: 3 } } - only update `bar.foo`, 
		// }
	);

// nested update
	const book = await em.findOneOrFail(Book, 1, {
			 populate: ['author'] 
			 // MUST populate, or new entry is created
		 });
	
	// update existing book's author's name
	wrap(book).assign({
	  author: {
	    id: book.author.id,
	    name: 'New name...',
	  },
	}, 
	{ updateByPrimaryKey: false } 
	// dont have to provide PK
	// without this new entity will be created
	);
	// addresses: [new Address(...)] -> reset collection
	// addresses: new Address(...) -> add to collection
```
- [ ] `mergeEmbeddedProperties`
- [ ] [IWrappedEntity](https://mikro-orm.io/docs/entity-helper#wrappedentity-and-wrap-helper)
	- Provided by `wrap` method or `BaseEntity` class
			- `warp` method returns `WrappedEntity` with public methods
			-  for internal properties `wrap(entity, true)`
### [Unit of Work](https://mikro-orm.io/docs/unit-of-work)
- Uses `Identity Map` pattern
	- improve performance by providing a context-specific, in-memory cache
	- prevents duplicate retrieval of the same object data from the database (a reference of object is kept inside `UnitOfWork`)
	- indexed by primary key (takes the performance advantage)
		- for other properties, returns same reference with database call
				- fetches the object from database
				- check if that object already exists
	- we don't have to call `em.persist()` for managed objects thanks to Identity Map
		- new entities are added after `em.persist`
			- POTENTIAL PROBLEM - `findOne` could return the unmanaged entity after persist
			- without PK in entity object that is auto flushed
- Data Mapper
	- Tries to achieve persistence-ignorance (JS object does not know relational database things)
	- Keeps a second map (inside `UnitofWork`) for all fetched objects
		- `em.flush()` call iterate through all entities and update the changed ones
### [Identity Map](https://mikro-orm.io/docs/identity-map)
- Improves performance for a query/request (doesn't help across request => different from cache)
- Helps reduce memory footprint
	- eg: Even keeps populated entries for findOne
- `em.clear` to clear identity map cache
###### [Request Context](https://mikro-orm.io/docs/identity-map#request-context)
- Helps with Dependency Injection (what NestJS does) - as it always provides same instance 
- TODO- check for other details
### [Collections](https://mikro-orm.io/docs/collections)
- Use `for of`to iterate, `getItems()` to access
	- `toArray()` to readonly serialize
- Array access for only read (doesn't check initialization, could throw error)
- [ ]  A lot of methods
- [ ] `Collection.Remove()` does not remove from database by default
```ts
// One to Many
	@OneToMany(() => Book, book => book.author)  
	// @OneToMany({ entity: () => Book, mappedBy: 'author' })  
	books2 = new Collection<Book>(this);
// Many to Many
	// Unidirectional
		@ManyToMany(() => Book)  
		@ManyToMany({ entity: () => Book, owner: true })  
		books2 = new Collection<Book>(this);
	// Bidirectional
		@ManyToMany(() => BookTag, tag => tag.books, { owner: true })  
		// @ManyToMany({ entity: () => BookTag, inversedBy: 'books' })  
		tags = new Collection<BookTag>(this);
		// -- inverse side
		@ManyToMany(() => Book, book => book.tags)
		// @ManyToMany({ entity: () => Book, mappedBy: 'tags' })
		books = new Collection<Book>(this);
			
```
- [ ] [Pivot table entity](https://mikro-orm.io/docs/collections#custom-pivot-table-entity)

### [Query Conditions](https://mikro-orm.io/docs/query-conditions)


## Questions
- [ ] what is Reference/wrapper
- [ ] `[HiddenProps]?: 'firstName' | 'lastName';` what this does?
- [ ]  tf is opaque string
- [ ] tf is pivot entity



- [ ]  how are we handling request context 