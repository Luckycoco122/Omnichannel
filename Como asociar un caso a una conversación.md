Como asociar un caso a una conversación
Las reglas de identificación tienen comportamientos estandar por lo que tendremos que modificarlas.
Para acceder a ellas tendremos abrir la herramienta Toolbox sql 4 

Haremos la consulta para obtener la columna msdyn_recordidentificationrule 
select msdyn_recordidentificationrule from msdyn_liveworkstream where msdyn_liveworkstreamid = 'GUID de la secuencia de Trabajo'
Opteniendo el estandar 
<RecordIdentificationRuleSet>
<RecordIdentificationRule><PrimaryEntity LogicalCollectionName="accounts" PrimaryKeyAttribute="accountid" PrimaryNameAttribute="name"/>
<fetch version="1.0" output-format="xml-platform" mapping="logical" top="2">
<entity name="account"><attribute name="accountid" />
<attribute name = "name" /><filter type="and">
<condition attribute="statuscode" operator="eq" value="1" />
<condition attribute="name" operator="eq" value="${Name}" />
<condition attribute="telephone1" operator="eq" source="msdyn_msdyn_ocliveworkitem_msdyn_ocphonecallengagementctx_liveworkitemid" value="${msdyn_fromphone}" />
<condition attribute="emailaddress1" operator="eq" value="${Email}" />
</filter></entity>
</fetch>
<ContextKey name="msdyn_account_msdyn_ocliveworkitem_Customer" isPreferred="false"/>
</RecordIdentificationRule> 
<RecordIdentificationRule><PrimaryEntity LogicalCollectionName="contacts" PrimaryKeyAttribute="contactid" PrimaryNameAttribute="fullname"/>
<fetch version="1.0" output-format="xml-platform" mapping="logical" top="2">
<entity name="contact">
<attribute name="contactid" />
<attribute name = "fullname" />
<filter type="and">
<condition attribute="statuscode" operator="eq" value="1" />
<condition attribute="fullname" operator="eq" value="${Name}" />
<condition attribute="mobilephone" operator="eq" source="msdyn_msdyn_ocliveworkitem_msdyn_ocphonecallengagementctx_liveworkitemid" value="${msdyn_fromphone}" />
<condition attribute="emailaddress1" operator="eq" value="${Email}" />
</filter></entity>
</fetch>
<ContextKey name="msdyn_contact_msdyn_ocliveworkitem_Customer" isPreferred="true"/>
</RecordIdentificationRule>
<RecordIdentificationRule> 
<PrimaryEntity LogicalCollectionName="incidents" PrimaryKeyAttribute="incidentid" PrimaryNameAttribute="title"/>
<fetch version="1.0" output-format="xml-platform" mapping="logical" top="2"> 
<entity name="incident"><attribute name="incidentid" />
<attribute name = "title" />
<filter type="and">
<condition attribute="ticketnumber" operator="eq" value="${CaseNumber}" />
<condition attribute="statuscode" operator="eq" value="1" />
<filter type="or"><filter type="and">
<condition attribute="statuscode" operator="eq" value="1" entityname="ac" />
<condition attribute="name" operator="eq" value="${Name}" entityname="ac" />
<condition attribute="telephone1" operator="eq" source="msdyn_msdyn_ocliveworkitem_msdyn_ocphonecallengagementctx_liveworkitemid" value="${msdyn_fromphone}" entityname="ac" />
<condition attribute="emailaddress1" operator="eq" value="${Email}" entityname="ac" />
</filter><filter type="and">
<condition attribute="statuscode" operator="eq" value="1" entityname="co" />
<condition attribute="fullname" operator="eq" value="${Name}" entityname="co" />
<condition attribute="mobilephone" operator="eq" source="msdyn_msdyn_ocliveworkitem_msdyn_ocphonecallengagementctx_liveworkitemid" value="${msdyn_fromphone}" entityname ="co" />
<condition attribute="emailaddress1" operator="eq" value="${Email}" entityname="co" />
</filter></filter></filter>
<link-entity name="account" from="accountid" to="customerid" link-type="outer" alias="ac" />
<link-entity name="contact" from="contactid" to="customerid" link-type="outer" alias="co" />
</entity></fetch><ContextKey name="msdyn_incident_msdyn_ocliveworkitem" />
</RecordIdentificationRule></RecordIdentificationRuleSet>

una vez me halla traido todo tengo que actualizar el codigo con las condiciones que necesite 
Una vez modificado el codigo volveremos a la herramienra ToolBox SQL 4 

y haremos un UpDate:
UPDATE msdyn_liveworkstream
SET msdyn_recordidentificationrule = 'NuevoValor'
WHERE msdyn_liveworkstreamid = 'GUID de la secuencia de Trabajo';


una vez echo comprobaremos que se han realizado los cambios correctamente haciendo la consulta del principio
