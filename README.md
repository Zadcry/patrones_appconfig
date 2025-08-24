# Examen Patrones Corte 1

# Integrantes:
1. Santiago Andrés Araque
2. Camilo Arciniegas Guerrero
3. Sergio Nieto Meneses
4. Julián Mauricio Zafra 

## Contenido

1. [Instalación Manual con Helm](#instalación-manual-con-helm)
2. [Configuración de ArgoCD](#configuración-de-argocd)
3. [Endpoints de acceso](#endpoints-de-acceso)

## **Instalación Manual con Helm**

### **Pasos de Instalación**

1. **Clonar el repositorio:**
   ```bash
    git clone <https://github.com/Zadcry/patrones_appconfig.git>
   ``` 
   E ir a la carpeta en la que se clonó el repositorio
2. **Creación de carpetas:** en la carpeta raíz, se usa el comando
  ```bash
    helm dependency update
  ```
  Se creará la carpeta de charts, a la vez que archivos como Chart.yaml (donde irá la configuración) y values.yaml
  
3. **Creación de carpetas específicas**: crear carpetas hasta que quede una ruta como las siguientes
```bash
    charts/pedido-app/charts/backend
    charts/pedido-app/charts/frontend
 ```
  Y en las dos aplicar el siguiente comando
  ```bash
    helm dependency update
  ```
  Esto crea archivos .yaml y templates en cada carpeta

## **Configuración de ArgoCD**

### **Estrategia de Sincronización**

ArgoCD está configurado con **sincronización automática** que:

- **Monitorea continuamente** el repositorio `https://github.com/Zadcry/patrones_appconfig.git`
- **Detecta cambios** cada 3 minutos ya que este es el intervalo por defecto
- **Aplica** automáticamente los cambios al cluster

### **Configuraciones de source**

```yaml
  source:
    repoURL: 'https://github.com/Zadcry/patrones_appconfig.git'
    targetRevision: HEAD
    path: charts/pedido-app
    helm:
      valueFiles:
      - values.yaml
      - values-prod.yaml
```

### **Configuraciones de Sync Policy**

```yaml
syncPolicy:
  automated:
    prune: true
    selfHeal: true
```

### **La configuración completa de ArgoCD está en las rutas:**

1. Desarrollo: 
```bash
examen-patrones/patrones_appconfig/environments/dev/application.yaml 
```
2. Producción:
```bash
examen-patrones/patrones_appconfig/environments/prod/application.yaml
```

## **Endpoints de acceso**

### Con IP Pública:
```bash
1. Frontend: 138.197.252.199/
2. Backend: 138.197.252.199/api/
```

### Con dominios - Desarrollo:
```bash
1. Frontend: dev.patrones.com/
2. Backend: dev.patrones.com/api/
```

### Con dominios - Producción:
```bash
1. Frontend: prod.patrones.com/
2. Backend: prod.patrones.com/api/
```
