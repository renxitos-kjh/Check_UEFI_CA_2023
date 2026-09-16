# Check_UEFI_CA_2023
# UEFI CA 2023 Check

Verifica si el equipo tiene instalados los certificados **Windows UEFI CA 2023** y **Microsoft Corporation KEK 2K CA 2023** en el firmware UEFI (Secure Boot). Requeridos para actualizaciones de Windows a partir de junio 2026.

Detecta automáticamente el idioma del sistema (Español / English) y no requiere configuración.

## Requisitos

- Windows 10/11 con **Secure Boot habilitado** en UEFI.
- PowerShell 5.1+ (incluido por defecto).
- Privilegios de administrador (el script se auto-eleva vía UAC).

## Uso

1. Ejecuta `Check_UEFI_CA_2023.exe` (doble clic).
2. Acepta el prompt de UAC si aparece.
3. Espera el resultado en consola:
   - `[OK]` verde → certificado presente.
   - `[X]` rojo → certificado ausente.
   - `[!]` amarillo → falta al menos uno.
4. La ventana se cierra sola a los 5 segundos, o revisa el log.

## Log

Se genera en:
```
%USERPROFILE%\Documents\UEFI_CA_2023_Check.log
```
Contiene el mismo resultado que la consola, con equipo, usuario, fecha y hora — útil para auditoría o soporte remoto.

## Interpretación de resultados

| Resultado | Significado | Acción |
|---|---|---|
| Ambos `[OK]` | Firmware listo para las actualizaciones de junio 2026 | Ninguna |
| Uno o ambos `[X]` | Falta actualizar certificados UEFI | Actualizar firmware/BIOS desde el fabricante, o esperar el rollout de Microsoft vía Windows Update |
| `[X] Error leyendo UEFI db/KEK` | Secure Boot deshabilitado o no soportado | Habilitar Secure Boot en BIOS/UEFI y volver a ejecutar |

## Notas técnicas

- Usa el cmdlet `Get-SecureBootUEFI` de PowerShell; **no funciona** si Secure Boot está deshabilitado o en modo Legacy/CSM.
- Si el `.exe` no eleva automáticamente, verifica que se haya compilado con el manifiesto `requireAdministrator` (ver configuración del compilador bat→exe usado).
- El script no modifica nada: es de solo lectura/diagnóstico.

## Solución de problemas

- **"No se pudo elevar privilegios"** → ejecuta manualmente como administrador (clic derecho → Ejecutar como administrador).
- **Error en ambos certificados** → confirma que Secure Boot esté en `Enabled` desde el BIOS/UEFI (tecla F2/DEL/F10 al arrancar, varía por fabricante).
- **El log no se genera** → verifica permisos de escritura en la carpeta Documentos del usuario actual.
