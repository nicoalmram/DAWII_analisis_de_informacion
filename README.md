# DAWII_analisis_de_informacion
Para la realización de esta práctica primero he tenido que preparar el entorno de Neo4j para poder leer los archivos csv en este caso. Para ello hemos hecho lo siguiente:
1.- Crear la base de datos que usaremos en Neo4j Desktop
2.- Instalar las librerías APOC y Graph Data Science Library en dicha base de datos.
3.- Añadir la línea "apoc.import.file.enabled=true" al final del fichero "neo4j.conf"
4.- Añadir los ficheros csv a la base de datos y a su carpeta import.

Una vez hecho todo esto, ya estamos listos para empezar la práctica.

Lo primero será cargar los ficheros csv a nuestra base de datos con el siguiente comando:

CALL apoc.import.csv(
  [{fileName: 'file:/pesca.csv', labels: ['SOLO']}],
  [{fileName: 'file:/pescaconjunta.csv', type: 'GRUPO'}],
  {delimiter: '|', arrayDelimiter: ',', stringIds: false}
)

Hecho esto, solo nos quedaría seleccionar uno de los algoritmos propuestos por la asignatura y probarlo con nuestros datos. En nuestro caso hemos seleccionado el algoritmo de centralidad "centralidad de cercanía":

CALL gds.alpha.closeness.stream({
  nodeProjection: 'SOLO',
  relationshipProjection: 'GRUPO'
})
YIELD nodeId, centrality
RETURN gds.util.asNode(nodeId).name AS user, centrality
ORDER BY centrality DESC
