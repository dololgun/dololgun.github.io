## 소나큐브란?



## 체크스타일 소나큐브 플러그인

### 개요

소나큐브에서 체크스타일을 실행할 수 있도록 도와주는 플러그인이다. 즉, 이 플러그인이 있으면 우리가 작성한 체크스타일 설정을 그대로 소나큐브에 적용할 수 있다. 이 플러그인의 몇 가지 단점이 있는데 체크스타일의 config파일을 그대로 명명규칙의 문제와 인식하지 않는 xml tag로 인하여 잘 동작하지 않는다. 그리고 custom check를 반영하기 위해서는 직접 ruleset을 추가하기 위해 플러그인을 직접 수정하고 컴파일하여 반영해야 한다.

### 설치

플러그인 설치파일은 github에서 받을 수 있다. 언급한 것처럼, custom check를 소나큐브에 반영하기 위해서는 이 플러그인을 직접 수정해야 한다.

빌드된 파일 :  [checkstyle-sonar-plugin-4.27.jar](\\46.1.100.250\it고도화\91. 담당자\박상규\이건호빌드\checkstyle-sonar-plugin-4.27.jar) 

소스 :  [sonar-checkstyle-4.27.zip](sonar-checkstyle-4.27.zip) 

### 주요 동작 

#### importProfile

```java
final SMInputFactory inputFactory = initStax();
final RulesProfile profile = RulesProfile.create();
try {
    final Module checkerModule = loadModule(inputFactory.rootElementCursor(reader)
                                            .advance());

    for (Module rootModule : checkerModule.modules) {
        final Map<String, String> rootModuleProperties = new HashMap<>(
            checkerModule.properties);
        rootModuleProperties.putAll(rootModule.properties);

        if (StringUtils.equals(TREEWALKER_MODULE, rootModule.name)) {
            processTreewalker(profile, rootModule, rootModuleProperties, messages);
        }
        else {
            processModule(profile, CHECKER_MODULE + "/", rootModule.name,
                          rootModuleProperties, messages);
        }
    }

}
catch (XMLStreamException ex) {
    final String message = "XML is not valid: " + ex.getMessage();
    LOG.error(message, ex);
    messages.addErrorText(message);
}
return profile;

```

#### loadModule

소나큐브는 rule을 기반으로 profile을 생성하여 profile기준으로 검증을 수행한다. 

우리가 작성한 체크스타일 설정파일(xml)은 upload시 profile import과정을 통하여 profile이 등록되는데 이 과정이 매끄럽지 못하다.

일단 지원하는 태그가 `module`, `property`뿐이다. 체크 스타일은 `message`도 있는데 아직 지원하지 않는 것 같다.

```java
private Module loadModule(SMInputCursor parentCursor) throws XMLStreamException {
    final Module result = new Module();
    result.name = parentCursor.getAttrValue("name");
    final SMInputCursor cursor = parentCursor.childElementCursor();
    while (cursor.getNext() != null) {
        final String nodeName = cursor.getLocalName();
        if (MODULE_NODE.equals(nodeName)) {
            result.modules.add(loadModule(cursor));
        }
        else if ("property".equals(nodeName)) {
            final String key = cursor.getAttrValue("name");
            final String value = cursor.getAttrValue("value");
            result.properties.put(key, value);
        }
    }
    return result;
}
```

### processRule

```java
private void processRule(RulesProfile profile, String path, String moduleName, Map<String, String> properties, ValidationMessages messages) {
    final Rule rule;
    final String id = properties.get("id");
    final String warning;
    if (StringUtils.isNotBlank(id)) {
        rule = ruleFinder.find(RuleQuery.create().withRepositoryKey(CheckstyleConstants.REPOSITORY_KEY).withKey(id));
        warning = "Checkstyle rule with key '" + id + "' not found";

    }
    else {
        final String configKey = path + moduleName;
        rule = ruleFinder.find(RuleQuery.create().withRepositoryKey(CheckstyleConstants.REPOSITORY_KEY).withConfigKey(configKey));
        warning = "Checkstyle rule with config key '" + configKey + "' not found";
    }

    if (rule == null) {
        messages.addWarningText(warning);

    }
    else {
        final ActiveRule activeRule = profile.activateRule(rule, null);
        activateProperties(activeRule, properties);
    }
}
```