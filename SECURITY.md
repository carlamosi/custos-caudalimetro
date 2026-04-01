# 🔐 Seguridad - Custos Caudalímetro

## Política de Reporte de Vulnerabilidades

Si encuentras una vulnerabilidad de seguridad, **por favor NO la publiques en GitHub Issues**.

### Contacto Seguro
- Envía correo a (disponible en perfil de GitHub)
- Asunto: `[SECURITY] Vulnerabilidad en Custos Caudalimetro`
- Incluye: descripción, pasos para reproducir, impacto

---

## 🔐 Credenciales - GUÍA CRÍTICA

### ⚠️ NUNCA hagas esto:

```javascript
// ❌ INCORRECTO - NO HACER ESTO
#define FIREBASE_AUTH "gt0C2QtjtM688tUiNAVRU2MIdvy6hSqJs1jbWoBh"
#define WIFI_PASSWORD "LuisMar1412"

// En commits
git commit -m "Fix: agregué mis credenciales de Firebase"

// En comentarios de código
// Mi API key es AIzaSyC...
```

### ✅ CORRECTO - SIEMPRE hacer esto:

```bash
# 1. Crear archivo .env
cp .env.example .env

# 2. Agregar a .gitignore
echo ".env" >> .gitignore

# 3. Editar .env con valores reales
nano .env

# 4. En código, usar variables
#define FIREBASE_AUTH ENVIRONMENT_VARIABLE

# 5. Verificar antes de hacer push
git status  # Confirma que .env no está listado

# 6. Hacer commit
git commit -m "feat: added environment template"
```

---

## 🔄 Regenerar Credenciales (Si las comprometiste)

### Firebase

```
1. Firebase Console → Configuración del Proyecto
2. Base de Datos en Tiempo Real → Reglas de Seguridad
3. Edita el token:
   - Elimina el token viejo
   - Genera uno nuevo
4. Actualiza en Arduino code y .env
```

### WiFi

```
1. Router → Configuración
2. Cambia contraseña WiFi
3. Desconecta dispositivos antiguos
4. Actualiza contraseña en Arduino
```

---

## 🛡️ Reglas de Seguridad Firebase

### Configuración Recomendada

```json
{
  "rules": {
    "readings": {
      ".read": "root.child('device_key').val() == auth.token.device_key",
      ".write": "root.child('device_key').val() == auth.token.device_key"
    },
    "history": {
      ".read": "root.child('device_key').val() == auth.token.device_key",
      ".write": "root.child('device_key').val() == auth.token.device_key"
    }
  }
}
```

---

## 📋 Checklist de Seguridad Pre-Deploy

- [ ] `.env` está en `.gitignore`
- [ ] No hay credenciales en commits recientes
- [ ] Token Firebase no es visible en código público
- [ ] WiFi usa WPA2/WPA3 (no WEP)
- [ ] Firmware ESP32 está actualizado
- [ ] Reglas Firebase están configuradas
- [ ] HTTPS es forzado en dashboard web
- [ ] Backups de datos están habilitados

---

## 🔍 Auditoría de Código

### Verificar exposición de secretos

```bash
# Buscar credenciales en histórico
git log --all -i --grep="api.*key" --grep="token" --grep="password"

# Buscar patrones sospechosos
grep -r "FIREBASE_AUTH\|API_KEY" --include="*.cpp" --include="*.js"

# Verificar ficheros .env no committed
git check-ignore .env
```

---

## 🚨 Respuesta a Incidentes

Si descubres credenciales publicadas:

### Inmediatamente
1. **REGENERA TODAS las credenciales**
2. **Invalida tokens en Firebase Console**
3. **Revisa uso no autorizado**
4. **Contacta a soporte de servicios comprometidos**

### En 24-48 horas
1. Audita logs de acceso
2. Cambia contraseñas principales
3. Revisa cambios en código
4. Actualiza documentación

### Comunicación
1. Notifica a usuarios afectados
2. Documenta el incidente
3. Implementa medidas preventivas

---

## 📚 Referencias

- [OWASP Top 10](https://owasp.org/www-project-top-ten/)
- [Firebase Security Best Practices](https://firebase.google.com/docs/security)
- [ESP32 Security Guide](https://docs.espressif.com/projects/esp-idf/en/latest/security/)
- [Node.js Security](https://nodejs.org/en/docs/guides/security/)

---

**Última actualización:** 2026-04-01
**Versión:** 1.0
