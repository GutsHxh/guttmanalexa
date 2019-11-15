# guttmanalexa

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd;
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.example.howtodoinjava</groupId>
    <artifactId>sayHelloLambdaForAlexa</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <repositories>
        <repository>
            <id>alexa-skills-kit-repo</id>
            <url>file://${project.basedir}/repo</url>
        </repository>
    </repositories>
 
    <dependencies>
        <dependency>
            <groupId>com.amazon.alexa</groupId>
            <artifactId>alexa-skills-kit</artifactId>
            <version>1.5.0</version>
            <scope>compile</scope>
        </dependency>
        <dependency>
            <groupId>org.eclipse.jetty</groupId>
            <artifactId>jetty-server</artifactId>
            <version>9.0.6.v20130930</version>
            <scope>compile</scope>
        </dependency>
        <dependency>
            <groupId>org.eclipse.jetty</groupId>
            <artifactId>jetty-servlet</artifactId>
            <version>9.0.6.v20130930</version>
            <scope>compile</scope>
        </dependency>
        <dependency>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
            <version>1.2.17</version>
            <scope>compile</scope>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-api</artifactId>
            <version>1.7.10</version>
            <scope>compile</scope>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
            <version>1.7.10</version>
        </dependency>
        <dependency>
            <groupId>org.apache.commons</groupId>
            <artifactId>commons-lang3</artifactId>
            <version>3.4</version>
            <scope>compile</scope>
        </dependency>
        <dependency>
            <groupId>org.apache.directory.studio</groupId>
            <artifactId>org.apache.commons.io</artifactId>
            <version>2.4</version>
            <scope>compile</scope>
        </dependency>
        <dependency>
            <groupId>com.amazonaws</groupId>
            <artifactId>aws-lambda-java-core</artifactId>
            <version>1.0.0</version>
            <scope>compile</scope>
        </dependency>
        <dependency>
            <groupId>com.amazonaws</groupId>
            <artifactId>aws-lambda-java-log4j</artifactId>
            <version>1.0.0</version>
            <scope>compile</scope>
        </dependency>
        <dependency>
            <groupId>com.amazonaws</groupId>
            <artifactId>aws-java-sdk-dynamodb</artifactId>
            <version>1.9.40</version>
        </dependency>
    </dependencies>
 
 
    <build>
        <sourceDirectory>src</sourceDirectory>
        <resources>
            <resource>
                <directory>src/resources</directory>
            </resource>
        </resources>
        <pluginManagement>
            <plugins>
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-compiler-plugin</artifactId>
                    <version>3.3</version>
                    <configuration>
                        <source>1.7</source>
                        <target>1.7</target>
                    </configuration>
                </plugin>
 
            </plugins>
        </pluginManagement>
    </build>
 
</project>
package com.example.howtodoinjava.alexa;
 
import java.util.HashSet;
import java.util.Set;
import com.amazon.speech.speechlet.Speechlet;
import com.amazon.speech.speechlet.lambda.SpeechletRequestStreamHandler;
 
public class SayHelloRequestStreamHandler extends SpeechletRequestStreamHandler {
 
    private static final Set<String> supportedApplicationIds;
 
    static {
        /*
         * This Id can be found on https://developer.amazon.com/edw/home.html#/
         * "Edit" the relevant Alexa Skill and put the relevant Application Ids
         * in this Set.
         */
        supportedApplicationIds = new HashSet<String>();
        //supportedApplicationIds.add("[Add your Alexa Skill ID and then uncomment and ]";
        System.out.println("Supported app ids : " + supportedApplicationIds);
    }
 
    public SayHelloRequestStreamHandler() {
        super(new SayHelloSpeechlet(), supportedApplicationIds);
    }
 
    public SayHelloRequestStreamHandler(Speechlet speechlet, Set<String> supportedApplicationIds) {
        super(speechlet, supportedApplicationIds);
    }
}
package com.example.howtodoinjava.alexa;
 
import com.amazon.speech.slu.Intent;
import com.amazon.speech.speechlet.IntentRequest;
import com.amazon.speech.speechlet.LaunchRequest;
import com.amazon.speech.speechlet.Session;
import com.amazon.speech.speechlet.SessionEndedRequest;
import com.amazon.speech.speechlet.SessionStartedRequest;
import com.amazon.speech.speechlet.Speechlet;
import com.amazon.speech.speechlet.SpeechletException;
import com.amazon.speech.speechlet.SpeechletResponse;
import com.amazon.speech.ui.PlainTextOutputSpeech;
import com.amazon.speech.ui.Reprompt;
import com.amazon.speech.ui.SimpleCard;
 
public class SayHelloSpeechlet implements Speechlet
{
    public SpeechletResponse onLaunch(final LaunchRequest request, final Session session)
        throws SpeechletException
    {
        System.out.println("onLaunch requestId={}, sessionId={} " + request.getRequestId()
                            + " - " + session.getSessionId());
        return getWelcomeResponse();
    }
 
    public SpeechletResponse onIntent(final IntentRequest request, final Session session)
        throws SpeechletException
    {
        System.out.println("onIntent requestId={}, sessionId={} " + request.getRequestId()
                            + " - " + session.getSessionId());
 
        Intent intent = request.getIntent();
        String intentName = (intent != null) ? intent.getName() : null;
 
        System.out.println("intentName : " + intentName);
 
        if ("SayHelloIntent".equals(intentName)) {
            return getHelloResponse();
        } else if ("AMAZON.HelpIntent".equals(intentName)) {
            return getHelpResponse();
        } else {
            return getHelpResponse();
        }
    }
 
    /**
     * Creates and returns a {@code SpeechletResponse} with a welcome message.
     *
     * @return SpeechletResponse spoken and visual response for the given intent
     */
    private SpeechletResponse getWelcomeResponse()
    {
        String speechText = "Welcome to the Alexa World, you can say hello to me, I can respond." +
                             "Thanks, How to do in java user.";
 
        // Create the Simple card content.
 
        SimpleCard card = new SimpleCard();
        card.setTitle("HelloWorld");
        card.setContent(speechText);
 
        // Create the plain text output.
 
        PlainTextOutputSpeech speech = new PlainTextOutputSpeech();
        speech.setText(speechText);
 
        // Create reprompt
 
        Reprompt reprompt = new Reprompt();
        reprompt.setOutputSpeech(speech);
 
        return SpeechletResponse.newAskResponse(speech, reprompt, card);
    }
 
    /**
     * Creates a {@code SpeechletResponse} for the hello intent.
     *
     * @return SpeechletResponse spoken and visual response for the given intent
     */
    private SpeechletResponse getHelloResponse()
    {
        String speechText = "Hello how to do in java user. It's a pleasure to talk with you. "
                        + "Currently I can only say simple things, "
                        + "but you can educate me to do more complicated tasks later. Happy to learn.";
 
        // Create the Simple card content.
 
        SimpleCard card = new SimpleCard();
        card.setTitle("HelloWorld");
        card.setContent(speechText);
 
        // Create the plain text output.
 
        PlainTextOutputSpeech speech = new PlainTextOutputSpeech();
        speech.setText(speechText);
 
        return SpeechletResponse.newTellResponse(speech, card);
    }
 
    /**
     * Creates a {@code SpeechletResponse} for the help intent.
     *
     * @return SpeechletResponse spoken and visual response for the given intent
     */
    private SpeechletResponse getHelpResponse()
    {
        String speechText = "Hello user, You can say hello to me!";
 
        // Create the Simple card content.
 
        SimpleCard card = new SimpleCard();
        card.setTitle("HelloWorld");
        card.setContent(speechText);
 
        // Create the plain text output.
 
        PlainTextOutputSpeech speech = new PlainTextOutputSpeech();
        speech.setText(speechText);
 
        // Create reprompt
 
        Reprompt reprompt = new Reprompt();
        reprompt.setOutputSpeech(speech);
 
        return SpeechletResponse.newAskResponse(speech, reprompt, card);
    }
 
    public void onSessionStarted(final SessionStartedRequest request, final Session session)
        throws SpeechletException
    {
        System.out.println("onSessionStarted requestId={}, sessionId={} " + request.getRequestId()
                            + " - " + session.getSessionId());
    }
 
    public void onSessionEnded(final SessionEndedRequest request, final Session session)
        throws SpeechletException
    {
        System.out.println("onSessionEnded requestId={}, sessionId={} " + request.getRequestId()
                            + " - " + session.getSessionId());
    }
}
