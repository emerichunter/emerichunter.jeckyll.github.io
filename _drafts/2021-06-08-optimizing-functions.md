---
published: false
---
## Optimizing Functions 2

In this post, I'm going to tell you about optimizing functions (hence the title). I am going to give you some tips and tricks to keep the performance at the maximum. 

My main first point will be to explain some options from the CREATE FUNCTION and then some other approaches to get the best out of your functions (this applies to procedures as well)

https://www.postgresql.org/docs/12/xfunc-optimization.html

From this very short entry within the official documentation. There are some takeaways already. The first thing to know is the function type you are using. 

### Volatile is stealing your speed


It might not seem obvious to all the users, but the default option for type is VOLATILE. The reason for this is very practical. It is because it is the most versatile form of function. With it you can do anything in a database, even change the data within the tables.

* You can find this in the CREATE FUNCTION page: *A query using a volatile function will re-evaluate the function at every row where its value is needed.* In other words, this is bad for performance because there will be no caching the results. (ref https://www.postgresql.org/docs/current/xfunc-volatility.html)
* Indexes on VOLATILE functions are prohibited. No way to index the result to get faster query. 
* As a result of being the default behavior, any SQL developing software (such as PgAdmin or others) do not display the type and just assume the type is VOLATILE.

Tip #1
*Use explicit type.*
Tip #2
*Use the strictest type to the behavior you expect from your function. If the type is not permissive enough the database will tell you by throwing an error*
Tip #3
*Index whenever you can using the right kind of index (more about that in another post)*
Tip #4
*Tip #3 implies that the index will be replicated on the secondaries. Use this to your advantage. The index can lower the cost of the calculations made by the functions but could also just be used to balance CPU and memory usage to the standby servers (if no index is used).*




Enter text in [Markdown](http://daringfireball.net/projects/markdown/). Use the toolbar above, or click the **?** button for formatting help.