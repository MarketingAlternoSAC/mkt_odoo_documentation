### Campos principales

| Campo | Tipo / Relación | Descripción |
|-------|-----------------|-------------|
|id_user|Char|Usuario que realizó la solicitud de OCR. Por defecto guarda el ID del entorno actual (`self.env.user.id`).| 
|id_company|Many2one|Id de compañia del usuario que realizó la solicitud de OCR. Por defecto guarda el ID de la compañia del entorno actual (`self.env.company.id`).|
|processed_response|Text|Respuesta procesada obtenida del OCR.|
|raw_response|Text|Respuesta cruda obtenida del OCR.|
|status|Char|Estado de la respuesta del OCR.|  
|res_model|Char|Modelo en el cual se empleo el OCR.|
|attachment_id|Many2one|Documento PDF/Imagen que se envió a procesar al OCR.|
|error_message|Text|Mensaje de error obtenido del server.|
|time_result|Float|Tiempo que se tardo en obtener la respuesta del OCR.|