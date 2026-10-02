### Nuevo Driver Arquitectónico
| ID | Driver arquitectónico | Origen | ¿Por qué influye? |
| :--- | :--- | :--- | :--- |
| **DA06** | El sistema debe permitir modificar funcionalidades sin afectar innecesariamente otros módulos. | AC05-Mantenibilidad | Influye en la separación de responsabilidades, modularidad y dependencias internas. |

### Resumen de Drivers Priorizados
| Driver | Problema que plantea | Decisión que responde |
| :--- | :--- | :--- |
| **DA01-Escalabilidad** | Aumentarán usuarios en campañas | Monolito modular con posibilidad de escalamiento horizontal |
| **DA02-Rendimiento** | Habrá alta concurrencia | Incorporar caché y optimizar comunicación/procesamiento |
| **DA03-Seguridad** | Hay datos sensibles | Autenticación y autorización |
| **DA04-Pago externo** | Hay que comunicarse con una pasarela | Integración mediante API y adaptadores |
| **DA05-API REST** | Frontend/backend deben comunicarse mediante REST | Separar interfaz y backend mediante API REST |
| **DA06-Mantenibilidad** | Cambios no deben afectar otros módulos | Modularidad + Clean Architecture |