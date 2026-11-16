# Migrar la entrega a un fork oficial sin destruir el repositorio

La inspección del remoto y la API de GitHub confirmó `Gegdgt/Proyecto_Final_Desarrollo_Seguro`, público, **fork=false**. No cumple el requisito de fork oficial. No se cambió `origin`, historial, rama ni repositorio remoto. No se hizo commit/push.

## Procedimiento pendiente del propietario

1. Abrir [juice-shop/juice-shop](https://github.com/juice-shop/juice-shop) → **Fork** → propietario `Gegdgt`. Elegir un nombre libre, por ejemplo `juice-shop-desarrollo-seguro`, y crear el fork público. No usar el nombre del repositorio actual.
2. Comprobar que GitHub muestra **forked from juice-shop/juice-shop** y visibilidad pública. Registrar URL y commit upstream.
3. Según el kit, deshabilitar los workflows upstream: Settings → Actions → General → deshabilitar Actions si se permite, o deshabilitar cada workflow en Actions. Si el fork ya los deja deshabilitados, conservar ese estado y documentarlo; no pulsar habilitación. Confirmar con docente qué workflows se necesitarán en H2.
4. Clonar en una **carpeta hermana nueva**, no dentro ni encima del proyecto actual:

```powershell
cd ..
git clone https://github.com/Gegdgt/juice-shop-desarrollo-seguro.git
cd juice-shop-desarrollo-seguro
git remote add upstream https://github.com/juice-shop/juice-shop.git
git remote -v
git switch -c docs/hito-1-diseno-seguro
```

Usar el nombre real elegido si difiere. Leer el `AGENTS.md` y las instrucciones upstream antes de editar el nuevo clon.

5. Inspeccionar destino y comprobar que `docs/hito-1`, `entorno-blue-team`, `scripts/validar-h1.py` y `scripts/recopilar-evidencia-h1.py` no existan. Copiar esas rutas de la copia actual; si hay colisiones, comparar y fusionar manualmente. No copiar `.git`, dependencias, archivos de ejecución ni credenciales.
6. Conservar el README oficial del fork. Guardar el README académico como `README-ACADEMICO.md` en la raíz nueva y añadir un enlace desde el README upstream. Al estar en la misma raíz, sus enlaces relativos conservan destino. Ajustar referencias al README académico donde corresponda.
7. **Fusionar** las reglas de `.gitignore`, sin reemplazar el archivo upstream. Preservar fixtures y claves deliberadas del proyecto original; no ignorar todo `.key`, `.db` o `.json`. Revisar también las reglas upstream que puedan ocultar entregables nuevos.
8. Actualizar URL de clonación, estado del fork y commit real. La imagen del laboratorio está fijada por digest y no se construye desde el fork: no afirmar que el commit clonado coincide con esa imagen sin comprobarlo. Para H2 alinear versión de análisis/imagen explícitamente.
9. Desde el clon nuevo repetir validación de documentos, Compose y HTTP. Detener primero el laboratorio de la copia anterior con `stop` o `down`, según necesidad de conservar contenedores. Ambos usan el nombre de proyecto `juiceshop-h1` y no deben gestionarse simultáneamente desde distintas copias.
10. Revisar `git status`, `git diff`, evidencia y exclusiones. Solo tras autorización explícita hacer commit/push al **fork propio**; no abrir PR al upstream para la entrega universitaria.

Para revisión previa a una futura publicación autorizada:

```powershell
git status --short
git diff --check
git diff
python .\scripts\validar-h1.py
docker compose -f .\entorno-blue-team\compose.yaml config
```

**Criterio de cierre:** URL pública del fork, relación upstream verificable, workflows en el estado exigido, entregables presentes y revisión del equipo. El repositorio original permanece intacto como historial académico.

Referencia: [GitHub — crear un fork](https://docs.github.com/en/pull-requests/how-tos/work-with-forks/fork-a-repo).
