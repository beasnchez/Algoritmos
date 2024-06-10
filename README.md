class Algoritmo:
    def buscar(self, lista, objetivo):
        for i, elemento in enumerate(lista):
            if elemento == objetivo:
                return i
        return -1

    def dividir_y_vencer(self, lista):
        if len(lista) == 0:
            return []
        if len(lista) == 1:
            return lista

        mitad = len(lista) // 2

        izquierda = self.dividir_y_vencer(lista[:mitad])
        derecha = self.dividir_y_vencer(lista[mitad:])

        return self.combinar(izquierda, derecha)

    def combinar(self, izquierda, derecha):
        i = 0
        j = 0
        resultado = []

        while i < len(izquierda) and j < len(derecha):
            if izquierda[i] < derecha[j]:
                resultado.append(izquierda[i])
                i += 1
            else:
                resultado.append(derecha[j])
                j += 1

        while i < len(izquierda):
            resultado.append(izquierda[i])
            i += 1

        while j < len(derecha):
            resultado.append(derecha[j])
            j += 1

        return resultado

    def voraz(self, lista):
        resultado = []

        lista.sort()  

        for elemento in lista:
            if self.es_valido(resultado, elemento):
                resultado.append(elemento)

        return resultado

    def es_valido(self, resultado, elemento, objetivo):
        if elemento < objetivo:
            return True
        else:
            return False

    def dijkstra(self, grafo, origen):
        distancias = {nodo: float('inf') for nodo in grafo}
        distancias[origen] = 0
        heap = [(0, origen)]
        
        while heap:
            distancia_actual, nodo_actual = heapq.heappop(heap)
            
            if distancia_actual > distancias[nodo_actual]:
                continue
            
            for vecino, peso in grafo[nodo_actual].items():
                distancia = distancia_actual + peso
                
                if distancia < distancias[vecino]:
                    distancias[vecino] = distancia
                    heapq.heappush(heap, (distancia, vecino))
        
        return distancias

    def floyd(self, grafo):
        n = len(grafo)
        distancias = [[float('inf')] * n for _ in range(n)]

        for i in range(n):
            for j in range(n):
                if i == j:
                    distancias[i][j] = 0
                elif grafo[i][j] != float('inf'):
                    distancias[i][j] = grafo[i][j]

        for k in range(n):
            for i in range(n):
                for j in range(n):
                    distancias[i][j] = min(distancias[i][j], distancias[i][k] + distancias[k][j])

        return distancias

    def mergesort(self, lista):
        if len(lista) <= 1:
            return lista

        mitad = len(lista) // 2
        izquierda = lista[:mitad]
        derecha = lista[mitad:]

        izquierda = self.mergesort(izquierda)
        derecha = self.mergesort(derecha)

        return self.combinar(izquierda, derecha)

    def combinar(self, izquierda, derecha):
        resultado = []
        i = 0
        j = 0

        while i < len(izquierda) and j < len(derecha):
            if izquierda[i] <= derecha[j]:
                resultado.append(izquierda[i])
                i += 1
            else:
                resultado.append(derecha[j])
                j += 1

        while i < len(izquierda):
            resultado.append(izquierda[i])
            i += 1

        while j < len(derecha):
            resultado.append(derecha[j])
            j += 1

        return resultado

    def heapsort(self, lista):
        n = len(lista)

        for i in range(n // 2 - 1, -1, -1):
            self.heapify(lista, n, i)

        for i in range(n - 1, 0, -1):
            lista[i], lista[0] = lista[0], lista[i]  # Intercambiar el elemento máximo con el último
            self.heapify(lista, i, 0)

        return lista

    def heapify(self, lista, n, i):
        mayor = i
        izquierda = 2 * i + 1
        derecha = 2 * i + 2
        if izquierda < n and lista[izquierda] > lista[mayor]:
            mayor = izquierda
        if derecha < n and lista[derecha] > lista[mayor]:
            mayor = derecha
        if mayor != i:
            lista[i], lista[mayor] = lista[mayor], lista[i]
            self.heapify(lista, n, mayor)
