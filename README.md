---


---

<h1 id="developer-blog">Developer Blog</h1>
<h2 id="th-january-2019">27th January 2019</h2>
<h3 id="creating-the-main-database">Creating the Main Database</h3>
<p>The first stage of security comes from creating an account on our Password Manager that all your passwords will be tied to. But, to allow the user to be able to register and log in to program, we need a database to store their credentials. We chose to use an SQLite database as it is embedded in the program as oppose to being a client-server database engine. This meant that we didn’t need a database server, instead the database is saved locally within the program (for now). In order to work with the SQLite database we needed to use sqlite-jdbc, a Java SQLite library. This library allows us to connect to the database and manipulate it as required e.g. adding a new record to a table.</p>
<p>To keep the code organised and easy to maintain, we created a new class called <em>DatabaseManager</em>. We decided to make the class non-static which means it has to be instantiated as a new object before being used. This meant that we could create a new object for every database we wished to connect to (for future use). The constructor code is shown below:</p>
<pre class=" language-java"><code class="prism  language-java"><span class="token keyword">public</span> <span class="token function">DatabaseManager</span><span class="token punctuation">(</span>String url<span class="token punctuation">)</span> <span class="token punctuation">{</span>
        <span class="token keyword">try</span> <span class="token punctuation">{</span>
            conn <span class="token operator">=</span> DriverManager<span class="token punctuation">.</span><span class="token function">getConnection</span><span class="token punctuation">(</span><span class="token string">"jdbc:sqlite:"</span> <span class="token operator">+</span> url<span class="token punctuation">)</span><span class="token punctuation">;</span>
            System<span class="token punctuation">.</span>out<span class="token punctuation">.</span><span class="token function">println</span><span class="token punctuation">(</span><span class="token string">"Connection Sucessful."</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
        <span class="token punctuation">}</span> <span class="token keyword">catch</span> <span class="token punctuation">(</span><span class="token class-name">SQLException</span> e<span class="token punctuation">)</span> <span class="token punctuation">{</span>
            System<span class="token punctuation">.</span>out<span class="token punctuation">.</span><span class="token function">println</span><span class="token punctuation">(</span>e<span class="token punctuation">.</span><span class="token function">getMessage</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
        <span class="token punctuation">}</span>
    <span class="token punctuation">}</span>
</code></pre>
<p>What this means is that when we create a new <em>DatabaseManager</em>, we have to specify the relative url of the database as parameter as show below:</p>
<pre class=" language-java"><code class="prism  language-java">DatabaseManager db <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">DatabaseManager</span><span class="token punctuation">(</span><span class="token string">"users.db"</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
</code></pre>
<p>Then the program will attempt to connect to the database (or create a new one if it doesn’t exist) and save the connection to the variable <em>conn</em>. Should any errors occur, they will be outputted to the console.</p>
<h3 id="querying-the-database-creating-the-table">Querying the Database: Creating the Table</h3>
<p>SQL (Structured Query Language) is a high-level programming language that is used to manipulate databases. In conjunction with <em>sqlite-jdbc</em>, we used SQL to create the main table of users using the following SQL statement.</p>
<pre class=" language-sql"><code class="prism  language-sql"><span class="token keyword">CREATE</span> <span class="token keyword">TABLE</span> <span class="token keyword">IF</span> <span class="token operator">NOT</span> <span class="token keyword">EXISTS</span> users <span class="token punctuation">(</span>id <span class="token keyword">integer</span> <span class="token keyword">PRIMARY</span> <span class="token keyword">KEY</span><span class="token punctuation">,</span>email <span class="token keyword">VARCHAR</span><span class="token punctuation">,</span>password <span class="token keyword">VARBINARY</span><span class="token punctuation">,</span> salt <span class="token keyword">VARBINARY</span><span class="token punctuation">)</span>
</code></pre>
<p><code>CREATE TABLE IF NOT EXISTS users</code> creates a table called <em>users</em> if another table with the same name doesn’t already exist in the database. We need this because the table will be created when the program is run for the first, but we don’t want errors being thrown the next time the program is run.</p>
<p><code>(id integer PRIMARY KEY,email VARCHAR,password VARBINARY, salt VARBINARY)</code> creates 4 columns in the table - <em>id</em>, <em>email</em>, <em>password</em> and <em>salt</em>.<br>
<code>id integer PRIMARY KEY</code> means that the column called <em>id</em> accepts integers (whole numbers) values only and is the primary key which means that the id value automatically generated per record starting from 1.<br>
<code>VARCHAR</code> means that the value held must be a string (alphanumeric) and the length can vary.<br>
<code>VARBINARY</code> means that the value held must be binary and the length can vary.</p>
<h3 id="creating-the-gui">Creating the GUI</h3>
<p>Now, that the database has been created, we need some sort of user interface so that the user can interact with the program. We used NetBeans’ GUI Builder to create the GUI for a program. It allowed us to easily build the GUI using drag-and-drop tools and not have to worry about the code. A sample is shown below.</p>

