---
layout: default
title: Git y GitHub
---

## ¿Qué es Git y por qué importa?

Git es el sistema de control de versiones más usado en el mundo. Te permite registrar cada cambio en tu código, volver a versiones anteriores y colaborar con otros sin pisarse el trabajo. Lo creó Linus Torvalds (el mismo de Linux) y es open source.

Sin Git, los equipos terminaban con archivos llamados `proyecto_final_v2_ESTE_SI.zip`. Con Git eso desaparece.

---

## Configuración inicial

Verificar que Git está instalado:

```bash
git --version
```

Configurar tu identidad (esto aparece en cada commit que hagas):

```bash
git config --global user.name "Tu Nombre"
git config --global user.email "tu@email.com"
```

Ver toda tu configuración actual:

```bash
git config --list
```

---

## Crear un repositorio desde cero

```bash
mkdir mi-proyecto       # crea la carpeta
cd mi-proyecto          # entra a la carpeta
git init                # convierte la carpeta en repositorio Git
```

Por defecto Git crea una rama llamada `master`. Si prefieres `main` (el estándar actual):

```bash
git config --global init.defaultBranch main   # para todos los repos futuros
git branch -m main                            # renombra la rama en el repo actual
```

---

## La diferencia entre Git y GitHub

**Git** es el programa que corre en tu computadora y registra los cambios.

**GitHub** es la plataforma en la nube donde subes tu código para compartirlo, colaborar con otros y tenerlo respaldado. Son cosas distintas: puedes usar Git sin GitHub, pero no GitHub sin Git.

## Referencias útiles

- [Documentación oficial de Git](https://git-scm.com/)
- [Cheat sheet de comandos Git](https://education.github.com/git-cheat-sheet-education.pdf)
- [Configurar Git por primera vez](https://git-scm.com/book/es/v2/Inicio---Sobre-el-Control-de-Versiones-Configurando-Git-por-primera-vez)