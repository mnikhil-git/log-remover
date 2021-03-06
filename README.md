log-remover
===========

This is a java agent that pre-processes byte-code before it is loaded by a class loader.

This goes through all the org.apache.hadoop.\* classes and removes calls to Log.debug() from them.

To test it, we run 4 million calls to Log.debug()

	$ java -javaagent:target/log-remover-1.0-SNAPSHOT.jar -jar target/log-remover-1.0-SNAPSHOT.jar
	We took 1 ms to do 4 M loops

and without the agent

	$ java  -jar target/log-remover-1.0-SNAPSHOT.jar 
	We took 33 ms to do 4 M loops

We do that by looking for an _invokeinterface_ opcode that is the LOG.debug(Object) call & replace it with a much cheaper _pop2_ operation. 

The hooks to do this are documented in the java.lang.instrument package (if tersely), in particular the ClassFileTransformer interface.
