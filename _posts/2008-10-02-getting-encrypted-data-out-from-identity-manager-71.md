---
id: 7
title: Getting EncryptedData out from Identity Manager 7.1
date: 2008-10-02T20:48:53+00:00
author: admin
layout: post
guid: http://www.darkedges.com/blog/?p=7
permalink: /2008/10/02/getting-encrypted-data-out-from-identity-manager-71/
categories:
  - Sun Identity Manager
tags:
  - Decrypt
  - Encrypt
  - Identity Manager
  - Password
---
Was working with a client wanting to do bulk imports with Sun Identity Manager 7.1 Service Provider Edition. They wanted to speed up User Provisioning by creating an LDIF to import direct into their Directory Instance, and not use the SPML interface and could not figure out how to encrypt the secret answers so that Identity Manager could decrypt them.

It seemed a good challenge and would through in some learning to boot, so I decided to try and reverse engineer the code. JAD helped a lot there, and soon enough I discovered the secrets to how the Encryption mechanism worked and quickly discovered that I would need to create a copy of a known class, and get 2 XML objects from the data store.

The class in question was <code>com.waveset.security.authn.ServerKeyStore</code>and the 2 XML objects needed where
<ol>
	<li>The Current Encryption Key being used</li>
	<li>The Miscellaneous object</li>
</ol>
The reason as to why <code>ServerKeyStore</code> was rewritten is due to the original code trying to connect to the Repository to get the necessary XML objects. The replacement code has these objects set manually, and therefore overrides allows me to fool the EncryptData class into using the values I have gained.

The Current <code><strong>EncryptionKey</strong></code> object holds an encrypted value using the <code><strong>PBEWithMD5AndDES</strong></code> cipher, and the <code><strong>Miscellaneous</strong></code> object holds Key to the Encryption Key. The <code><strong>EncryptionKey</strong></code> is pretty easy to import, as it can take a String to instantiate, <code><strong>Miscellaneous </strong></code>requires you to convert to an XML DOM and then extract an Element to use to instantiate itself.

The code is below, and there is an example given which should return <code><strong>dixon</strong></code>.

A lot of this could be changed to get the data directly from the Database, but for ease of use I have extracted the details manually.
<pre>[sourcecode language='java']
package com.darkedges.data;

import java.lang.reflect.InvocationTargetException;

import com.waveset.util.EncryptedData;
import com.waveset.util.WavesetException;

public class EncryptData {

	public static void main(String[] args) throws SecurityException,
			NoSuchMethodException, IllegalArgumentException,
			IllegalAccessException, InvocationTargetException, WavesetException {
		try {
			EncryptedData data = new EncryptedData();
			data
					.fromString("753B4AA4FD8AC224:-3951121B:11B967AC1B9:-7FFC|5DUTsMglZvA=");
			System.out.println(data.decryptToString());
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
[/sourcecode]</pre>
<pre>[sourcecode language='java']
package com.waveset.security.authn;

import java.io.IOException;
import java.io.StringReader;

import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.parsers.ParserConfigurationException;

import org.w3c.dom.Document;
import org.w3c.dom.Element;
import org.w3c.dom.NodeList;
import org.xml.sax.InputSource;
import org.xml.sax.SAXException;

import com.waveset.object.EncryptionKey;
import com.waveset.object.Miscellaneous;
import com.waveset.util.EncryptedData;
import com.waveset.util.Encryptor;
import com.waveset.util.WavesetException;

public class ServerKeyStore {
	private static EncryptionKey ek;
	private static Miscellaneous misc;
	private static byte _keyEncryptionKey[];
	static {
		try {
			ek = new EncryptionKey(
					"<EncryptionKey id='#ID#753B4AA4FD8AC224:-3951121B:11B967AC1B9:-7FFB' name='753B4AA4FD8AC224:-3951121B:11B967AC1B9:-7FFC' creator='createEncryptionKey' createDate='1218000373579' lastModifier='rencryptServerKeys' lastModDate='1218000373698' lastMod='1' value='PBEWithMD5AndDES|jMY74Tleq2CUf02URabaMBMdrpru31qEhJPpwGHQfD1Wtis0SgsZKcKPWmcdJxBRuaY9fEEogz0='><MemberObjectGroups><ObjectRef type='ObjectGroup' id='#ID#Top' name='Top'/></MemberObjectGroups></EncryptionKey>");
			misc = getMiscData();
		} catch (WavesetException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} catch (ParserConfigurationException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} catch (SAXException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} catch (IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		;

	}

	public ServerKeyStore() {
	}

	public static void getKey(String keyId) throws WavesetException {
		EncryptedData.setKey(getKeyBytes(keyId));
	}

	private static byte[] getKeyBytes(String keyId) throws WavesetException {
		return ek.getValue().decrypt();
	}

	public static String getCurrentKeyId() throws WavesetException {
		return ek.getName();
	}

	public static void getKeyEncryptionKey() throws WavesetException {
		EncryptedData
				.setKeyEncryptionKey(Encryptor.generateKey("PBEWithMD5AndDES", misc.getData().decryptToString()));
	}

	public static Miscellaneous getMiscData() throws ParserConfigurationException,
			SAXException, IOException, WavesetException {
		String xml = "<Miscellaneous id='#ID#753B4AA4FD8AC224:-3951121B:11B967AC1B9:-7FFA' name='miscData' creator='ServerKeyStore' createDate='1218000373660' lastModifier='ServerKeyStore' lastModDate='1218000373660' miscData='gSeTf4ayE5OJf0dfwzA4dIp0VyBVg6SGW/tVuZme8J+fBBkJrDKsWxe8bTZaV9wtbTsctgGxZhRab9rN6yndEVv7VbmZnvCfVALwAuUtyu7DEkApwCqzACzJzKaqZlOX2AuyG50vWpOaT2v9gt22U7ZuAtvMCAyGQq8iypghsro='><MemberObjectGroups><ObjectRef type='ObjectGroup' name='Top'/></MemberObjectGroups></Miscellaneous>";
		DocumentBuilderFactory factory = DocumentBuilderFactory.newInstance();
		DocumentBuilder db = factory.newDocumentBuilder();
		InputSource inStream = new InputSource();

		inStream.setCharacterStream(new StringReader(xml));
		Document doc = db.parse(inStream);
		NodeList nodeList = doc.getElementsByTagName("Miscellaneous");
		Element element = (org.w3c.dom.Element) nodeList.item(0);
		return new Miscellaneous(element);
	}
}
[/sourcecode]</pre>