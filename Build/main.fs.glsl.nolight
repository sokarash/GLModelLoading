#version 330 core
struct PointLight {
    vec3 position;  
  
    vec3 ambient;
    vec3 diffuse;
    vec3 specular;
    
    float constant;
    float linear;
    float quadratic;
}; 

out vec4 FragColor;

in vec2 TexCoords;

in vec3 FragPos;
in vec3 Normal;
uniform vec3 viewPos;

uniform sampler2D texture_diffuse1;
uniform sampler2D texture_specular1;

uniform PointLight pointLight;

vec3 CalcPointLight(PointLight light, vec3 normal, sampler2D samplerDiffuse, sampler2D samplerSpecular, vec3 fragPos, vec3 viewDir);;

void main()
{    
    //FragColor = texture(texture_diffuse1, TexCoords);

    vec3 norm = normalize(Normal);
    vec3 viewDir = normalize(viewPos - FragPos);
    FragColor = vec4(CalcPointLight(pointLight, norm, texture_diffuse1, texture_specular1, FragPos, viewDir), 1.0);
}

vec3 CalcPointLight(PointLight light, vec3 normal, sampler2D samplerDiffuse, sampler2D samplerSpecular, vec3 fragPos, vec3 viewDir)
{
    vec3 lightDir = normalize(light.position - fragPos);

    float diff = max(dot(normal, lightDir), 0.0);

    vec3 reflectDir = reflect(-lightDir, normal);
    float spec = pow(max(dot(viewDir, reflectDir), 0.0), 64.0);

    float distance = length(light.position - fragPos);
    float attenuation = 1.0 / (light.constant + light.linear * distance + light.quadratic * (distance * distance));

    vec3 ambient = light.ambient * vec3(texture(samplerDiffuse, TexCoords));
    vec3 diffuse = light.diffuse * diff * vec3(texture(samplerDiffuse, TexCoords));
    vec3 specular = light.specular * spec * vec3(texture(samplerSpecular, TexCoords));

    ambient *= attenuation;
    diffuse *= attenuation;
    specular *= attenuation;

    return (ambient + diffuse + specular);
}
