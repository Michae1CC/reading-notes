
pg 72
- Whether you store an ID or text string is a question of _normalization_. When you use an ID, your data is more normalized; the information that is meaningful to humans (such as _Washinton DC_) is stored in only one place, and everything that refers to it uses an ID (which has meaning only within the database). When you store text directly, you are duplicating the human-meaningful information in every record that uses it; this representation is denormalized.
- The process of looking up human-readable information by ID is called hydrating the IDs, and it is essentially a join performed in application code
- Main arguments in favour of document data are schema flexibility, better performance due to locality, and for some applications it is closer to the object model used by the application
- Main arguments in favour of relational models is providing better support for joins and many-to-one and many-to-many relationships