# jira_service_desk_cloud_api


Jira Service Desk Cloud Developer Info: https://developer.atlassian.com/cloud/jira/service-desk/

Iniciado de thisisdevelopment/jira_service_desk_api


## 1. Como usar esta API


Uso de cotraseñas con Jira REST API basic authentication

El uso de contraseñas(password) esta 

El uso de contraseñas (password) en la autenticación básica REST API está en desuso (Deprecated) y se eliminará en el futuro. Si bien la API REST de Jira actualmente acepta la contraseña de su cuenta de Atlassian en las solicitudes de autenticación básicas, le recomendamos que utilice un tokens de API. Ya que el uso de contraseña se eliminará en el futuro.

<pre>
public function __construct()
    {
        $this->jira = new JiraServiceDesk();
        $this->jira->setHost(env("JIRA_HOST",null));
        $this->jira->setUsername(env( "JIRA_USERNAME",null));
        $this->jira->setPassword(env("JIRA_TOKEN",null));
    }
</pre>

Para crear el Token de usuario debes ir a "Your perfil" -> "Security" en la dirección https://id.atlassian.com/manage-profile/security. Hacer clic en el enlace "Create and manage API tokens" para crear un nuevo Token. 

Usa ese token como contraseña de Jira Service Desk Api, junto con el email como username para un conexión con Jira REST API basic authentication. (Ver más en:https://developer.atlassian.com/cloud/jira/service-desk/jira-rest-api-basic-authentication/)

## 2. Ejemplos

Crear una nueva solicitud de servicio:
<pre>
    public function addTicket(int $serviceDeskID, int $requestTypeId, string $subject, string $message): string
    {
        
        $ticket =  new RequestModel();

        $ticket->setServiceDeskId($serviceDeskID);
        $ticket->setRequestSummary($subject);
        $ticket->setRequestDescription($message);
        $ticket->setRequestTypeId($requestTypeId);
       
        return response()->json($this->jira->request->createCustomerRequest($ticket));
    }

</pre>


Añadir un comentario a una solicitud de servicio:
<pre>
    public function addMessage(string $jiraTicketId, string $message, bool $isPublic):string
    {
        return response()->json($this->jira->request->createRequestComment($jiraTicketId,$message,$isPublic));
    }
</pre>


Obtener los datos de una solicitud:
<pre>
    public function retrieveTicket(string $jiraTicketId):string
    {
        return response()->json($this->jira->request->getCustomerRequestByIdOrKey($jiraTicketId));
    }
</pre>



## 3. Resumen de la API

<pre>
<table><thead><tr><th>Recurso</th><th>Descripción</th></tr></thead>
    <tbody>
        <tr><td>customer</td><td>Obtener a los clientes dentro de una instancia de Jira. Crear nuevos clientes.</td></tr>
        <tr><td>info</td><td>Proporciona detalles de la versión del software Jira Service Desk, las compilaciones y los enlaces relacionados..</td></tr>
        <tr><td>organization</td><td>Permite agrupar a los clientes de Jira Service Desk. Puedes crear y eliminar organizaciones, y agregar y eliminar clientes de ellas.</td></tr><tr><td>request</td><td>Gestiona las solicitudes de los clientes en en un proyecto (Desk Service). Puedes crear nuevas solicitudes y actualizar los detalles de la solicitud, como los archivos adjuntos y los comentarios. Tomar medidas para actualizar el estado de la solicitud o revisar el rendimiento del ANS (SLA).</td></tr><tr><td>requesttype</td><td>Permite obtener una lista de los tipos de solicitudes de los clientes, Clasificar las solicitudes en un proyecto.</td></tr><tr><td>servicedesk</td><td>Representa un proyecto o Desk Service. Puedes recuperar los proyectos en su instancia de Jira, administrar las solicitudes, administrar a los clientes y organizaciones asociados y recuperar los detalles de las colas de solicitudes...</td></tr>
    </tbody>
</table>
</pre>
