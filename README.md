# sharingData
import org.apache.http.client.methods.HttpPost;
import org.apache.http.entity.StringEntity;
import org.apache.http.impl.client.CloseableHttpClient;
import org.apache.http.impl.client.HttpClients;
import org.apache.http.util.EntityUtils;

public class HttpPostToCurlConverter {

    public static String convertHttpPostToCurl(HttpPost httpPost) throws Exception {
        StringBuilder curlCommand = new StringBuilder("curl -X POST ");

        // URL
        curlCommand.append(httpPost.getURI().toString());

        // Headers
        for (Header header : httpPost.getAllHeaders()) {
            curlCommand.append(" -H \"").append(header.getName()).append(": ").append(header.getValue()).append("\"");
        }

        // Entity (data)
        HttpEntity entity = httpPost.getEntity();
        if (entity != null) {
            String entityString = EntityUtils.toString(entity);
            curlCommand.append(" -d \"").append(entityString).append("\"");
        }

        return curlCommand.toString();
    }

    public static void main(String[] args) throws Exception {
        CloseableHttpClient httpclient = HttpClients.createDefault();
        HttpPost httpPost = new HttpPost("https://example.com/api/endpoint");
        httpPost.setHeader("Content-Type", "application/json");
        httpPost.setEntity(new StringEntity("{\"data\": \"value\"}"));

        String curlCommand = convertHttpPostToCurl(httpPost);
        System.out.println(curlCommand);

        httpclient.close();
    }
}
