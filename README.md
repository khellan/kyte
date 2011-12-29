# Kyte - a binding from Erlang to Kyoto Cabinet.

## Trivia

[Kyoto Cabinet](http://fallabs.com/kyotocabinet/) is an embeddable key-value storage.

Kyte uses NIFs to bind to it. 
Unpredictably long 'unmanaged' requests to the database are executed in the therad pool. 
This lets not to bother the BEAM scheduler.

## Usage

### Create a pool

Work with any Kyoto database is performed inside a thread pool.
You can create several thread pools to work with different databases relatively independently.

<code>
{ok, Pool} = kyte:pool_create(8). % Create a pool of eight threads. The pool is linked to the creating process.
</code>

### Open a database

Having a pool created you can open a database:

<code>
DbArgs = #kyte_db_args{
	file = "data/accounts.kch"
},
{ok, Db} = kyte:db_open(Pool, DbArgs). % Open a database. The database is linked to the creating process.
</code>

### Close the database

In order to manually close the database do as follows:

<code>
ok = kyte:db_close(Db).
</code>

Actually you do not often have to close your databases manually. 
They are linked and will close with the 'owning' process when it's being halted with the reason 'shutdown'.
