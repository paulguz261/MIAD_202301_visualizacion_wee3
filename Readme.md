# Documentacion modelado de datos

## Descripcion de variables

1. Variables en base de datos original.

| Nombre Original | tipo_dato | Descripción |
| --- | --- | --- |
|Timestamp|texto| Hora de la encuesta |
|How old are you?|texto| rango de edad del encuestado |
|What industry do you work in?|texto| categoria de industria donde trabaja el encuestado |
|Job title|texto| Nombre del trabajo |
|If your job title needs additional context, please clarify here:|texto| aclaraciones sobre el trabajo que desempeña en encuestado |
|What is your annual salary? (You'll indicate the currency in a later question. If you are part-time or hourly, please enter an annualized equivalent -- what you would earn if you worked the job 40 hours a week, 52 weeks a year.)| texto | salario anual del encuestado |
|How much additional monetary compensation do you get, if any (for example, bonuses or overtime in an average year)? Please only include monetary compensation here, not the value of benefits.|float64| ingresos adicionales como bonificaciones adicionales al salario |
|Please indicate the currency|texto| moneda en la que está el valor del salario anual |
|If "Other," please indicate the currency here: |texto| especificación para otras monedas en la que se suministra el salario |
|If your income needs additional context, please provide it here:|texto| aclaraciones sobre el ingreso reportado |
|What country do you work in?|texto| país en donde trabaja el encuestado |
|If you're in the U.S., what state do you work in?|texto| Si el país donde trabaja es EEUU especifique en qué estado (Ej. Florida)|
|What city do you work in?|texto| ciudad en donde trabaja el encuestado |
|How many years of professional work experience do you have overall?|texto| años de experiencia profesional, puede ser en otras industrias distintas a la indicada en la encuesta|
|How many years of professional work experience do you have in your field?|texto| años de experiencia profesional en la industria mencionada en la encuesta|
|What is your highest level of education completed?|texto| nivel de educación del encuestado|
|What is your gender?|texto| género del encuestado |
|What is your race? (Choose all that apply.)|texto| raza del encuestado|



2. Nombres de campos Homologados

| Nombre Original | Nombre Homologado |
| --- | --- |
|Timestamp|Timestamp|
|How old are you?|Age|
|What industry do you work in?|Industry|
|Job title|Job_title|
|If your job title needs additional context, please clarify here:|Job_additional_context|
|What is your annual salary? (You'll indicate the currency in a later question. If you are part-time or hourly, please enter an annualized equivalent -- what you would earn if you worked the job 40 hours a week, 52 weeks a year.)|annual_salary|
|How much additional monetary compensation do you get, if any (for example, bonuses or overtime in an average year)? Please only include monetary compensation here, not the value of benefits.|monetary_compensation|
|Please indicate the currency|currency|
|If "Other," please indicate the currency here: |other_currency|
|If your income needs additional context, please provide it here:|income_aditional_context|
|What country do you work in?|job_country|
|If you're in the U.S., what state do you work in?|us_state|
|What city do you work in?|job_city|
|How many years of professional work experience do you have overall?|professional_experience|
|How many years of professional work experience do you have in your field?|field_experience|
|What is your highest level of education completed?|highest_education_level|
|What is your gender?|gender|
|What is your race? (Choose all that apply.)|race|

3. Nuevos campos calculados

| Nombre Campo | Descripcion | tipo |
| --- | --- | --- |
|standard_currency| Estandarización de los valoers originales de la moneda   | Texto |
|standard_job_country| Estandarización del nombre del país donde trabaja el encuestado   | Texto |
|standard_us_state|  Estandarización del estado de EEUU donde trabaja el encuestado  | Texto |
|standard_job_city|  Estandarización de la ciudad donde trabaja el encuestado  | Texto |
|standard_annual_salary|  Estandarizacion del valor del ingreso reportado  | Numerico |
|total_income|  Suma del standard_annual_salary + monetary_compensation para obtener el total de ingresos por encuestado  | Numerico |
|currency_to_cop|  Valor del una unidad de la moneda reportada a cop Ej. 1 USD -> COP 4900  | Numerico |
|total_income_cop|  Conversión del total_income a COP total_income * currency_to_cop  |  Numerico |
|standard_annual_salary_cop|  Conversión a COP de la variable standard_annual_salary. standard_annual_salary * currency_to_cop  |  Numerico |
|monetary_compensation_cop|  Conversión a COP de la variable monetary_compensation. monetary_compensation* currency_to_cop |  Numerico |


## Proceso de actualizacion del reporte

1. Actualizacion de divisas

para actualizar las divisas se puede utilizar google sheets para generar una tabla de informacion actualizada, para ello generar un documento el cual debe tener las columnas 

* Name: codigo de 3 letras de las distintas divisas (ISO 4217)
* value_usd: formula de google sheets : =ifna(GOOGLEFINANCE(CONCATENATE("CURRENCY:",B3,"USD"))*A3,0)
* value_cop: =ifna(GOOGLEFINANCE(CONCATENATE("CURRENCY:USDCOP"))*C3,0)

**Ej.**

|Name|value_usd|value_cop|
| --- | --- | --- |
|GBP | 1.2055 | 5717.691322 |


una vez se tenga este documento se procede a descargarlo y guardarlo en la carpeta **data** con el nombre **currency.csv**

2. Estandarizacion de informacion 
Descargar el resultado de la encuesta y almacenarlo en la carpeta **data** del proyecto y verificar que tenga el nombre 
**Ask A Manager Salary Survey 2021 (Responses) - Form Responses 1.tsv**

Ejecutar el notebook en su totalidad, el cual consta de las siguientes secciones
- Estandarizar Nombres de campos
- Estandarización currency
- Estandarización Paises
- Estandarizar US states
- Estandarizacion ciudades
- Estandariza annual salary
- Union de tasas de cambio y caculo de salarios en pesos

3. Descargar la informacion de la encuesta estandarizada

    El procesamiento de los datos queda almacenadoen el archivo **salary_survey_cleaned.csv**

4. cargar la informacion en looker studio 