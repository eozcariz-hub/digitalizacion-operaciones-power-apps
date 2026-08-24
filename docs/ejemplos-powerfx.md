# Ejemplos de implementación con Power Fx

Esta sección presenta fórmulas representativas utilizadas en la aplicación para estandarizar datos, validar lecturas y controlar el envío de los formularios.

Las fórmulas utilizan la configuración regional de Power Apps en español, por lo que los argumentos se separan mediante punto y coma.

## Estandarización de niveles

Los niveles registrados durante las recorridas se limitan a valores predefinidos para evitar diferencias de escritura y facilitar su análisis posterior.

```powerfx
["Alto"; "Normal"; "Bajo"]
```

## Validación visual de rangos numéricos

Ejemplo aplicado al borde de un campo que admite valores entre 0 y 200:

```powerfx
If(
    Or(
        IsBlank(DataCardValue65.Text);
        Not(IsNumeric(DataCardValue65.Text));
        Value(DataCardValue65.Text) < 0;
        Value(DataCardValue65.Text) > 200
    );
    Color.Red;
    Color.Black
)
```

El borde cambia a rojo cuando el campo está vacío, contiene texto o presenta un valor fuera del rango permitido.

## Notificación de valores incorrectos

Ejemplo aplicado a la propiedad `OnChange` del campo:

```powerfx
If(
    Or(
        IsBlank(DataCardValue65.Text);
        Not(IsNumeric(DataCardValue65.Text))
    );
    Notify(
        "El valor no es numérico o está vacío";
        NotificationType.Error
    );
    If(
        Or(
            Value(DataCardValue65.Text) < 0;
            Value(DataCardValue65.Text) > 200
        );
        Notify(
            "Valor fuera de rango, colocar entre 0 y 200";
            NotificationType.Error
        );
        Notify(
            "Valor dentro del rango permitido";
            NotificationType.Success
        )
    )
)
```

## Control del botón Enviar datos

El botón permanece deshabilitado mientras existan valores inválidos o el campo Novedades esté vacío:

```powerfx
If(
    Or(
        IsBlank(DataCardValue65.Text);
        Not(IsNumeric(DataCardValue65.Text));
        Value(DataCardValue65.Text) < 0;
        Value(DataCardValue65.Text) > 200;

        IsBlank(DataCardValue64.Text);
        Not(IsNumeric(DataCardValue64.Text));
        Value(DataCardValue64.Text) < 0;
        Value(DataCardValue64.Text) > 30;

        IsBlank(DataCardValue189.Text);
        Not(IsNumeric(DataCardValue189.Text));
        Value(DataCardValue189.Text) < 0;
        Value(DataCardValue189.Text) > 160;

        IsBlank(DataCardValue190.Text);
        Not(IsNumeric(DataCardValue190.Text));
        Value(DataCardValue190.Text) < 0;
        Value(DataCardValue190.Text) > 160;

        IsBlank(DataCardValue66.Text)
    );
    DisplayMode.Disabled;
    DisplayMode.Edit
)
```

## Validación del campo Novedades

### Color del borde

```powerfx
If(
    IsBlank(DataCardValue66.Text);
    Color.Red;
    Color.Gray
)
```

### Notificación

```powerfx
If(
    IsBlank(DataCardValue66.Text);
    Notify(
        "Por favor, completa el campo Novedades";
        NotificationType.Error
    )
)
```

## Envío del formulario

```powerfx
SubmitForm(Turbinas_CD2);;
Notify(
    "Los datos han sido guardados";
    NotificationType.Success
)
```

Estas validaciones permiten reducir errores de carga, asegurar la integridad de los registros y generar información estructurada para su posterior transformación y análisis.
