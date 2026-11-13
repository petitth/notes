# SOAPUI

## Assertion scripts

### Check headers

```text
def status = messageExchange.responseHeaders["#status#"]
assert messageExchange.responseHeaders["#status#"] != null


log.info "'" + status[0].toString() + "'"
assert status[0].toString().equals('HTTP/1.1 200 ')
```

### Check xml as text

```text
import com.eviware.soapui.support.XmlHolder

def holder = new XmlHolder( messageExchange.responseContentAsXml )
def responseXml = holder.getPrettyXml()
assert responseXml.contains('Richard E. Silverman')

def title = holder.getNodeValue('//ns1:BooksResult/ns1:Books/ns1:CustomBookModel[2]/ns1:Title')
assert title.equals('Learning JavaScript Design Patterns')
```

### Check xml with XPath

```text
import com.eviware.soapui.support.XmlHolder

def holder = new XmlHolder( messageExchange.responseContentAsXml )

def lastName = holder.getNodeValue('//ns2:getUserByIdResponse/ns2:user/ns2:lastName')
def firstName = holder.getNodeValue('//ns2:getUserByIdResponse/ns2:user/ns2:firstName')
def country = holder.getNodeValue('//ns2:getUserByIdResponse/ns2:user/ns2:country')

assert lastName.equals('Anders')
assert firstName.equals('Maria')
assert country.equals('Germany')

if(country.equals('Germany'))
{ log.info "country is ok" }
else
{ log.info "country is NOT ok"}
```