---
title: Spring技术内幕——IoC容器的实现
date: 2016-10-31 18:08:08
tags: [java,spring]
---
[文章来源:Spring技术内幕——IoC容器的实现](http://blog.csdn.net/u011229848/article/details/52983669)


版权声明：本文为博主原创文章，未经博主允许不得转载。 https://blog.csdn.net/u011229848/article/details/52983669

对Spring IOC的理解离不开对依赖反转模式的理解，我们知道，关于如何反转对依赖的控制，把控制权从具体业务对象手中转交到平台或者框架中，是解决面向对象系统设计复杂性和提高面向对象系统可测试性的一个有效的解决方案。这个问题触发了IoC设计模式的发展，是IoC容器要解决的核心问题。同时，也是产品化的IoC容器出现的推动力。而我觉得Spring的IoC容器，就是一个开源的实现依赖反转模式的产品。

那具体什么是IoC容器呢？它在Spring框架中到底长什么样？说了这么多，其实对IoC容器的使用者来说，我们常常接触到的BeanFactory和ApplicationContext都可以看成是容器的具体表现形式。这些就是IoC容器，或者说在Spring中提IoC容器，从实现来说，指的是一个容器系列。这也就是说，我们通常所说的IoC容器，如果深入到Spring的实现去看，会发现IoC容器实际上代表着一系列功能各异的容器产品。只是容器的功能有大有小，有各自的特点。打个比方来说，就像是百货商店里出售的商品，我们举水桶为例子，在商店中出售的水桶有大有小；制作材料也各不相同，有金属的，有塑料的等等，总之是各式各样，但只要能装水，具备水桶的基本特性，那就可以作为水桶来出售来让用户使用。这在Spring中也是一样，它有各式各样的IoC容器的实现供用户选择和使用；使用什么样的容器完全取决于用户的需要，但在使用之前如果能够了解容器的基本情况，那会对容器的使用是非常有帮助的；就像我们在购买商品时进行的对商品的考察和挑选那样。
我们从最基本的XmlBeanFactory看起，它是容器系列的最底层实现，这个容器的实现与我们在Spring应用中用到的那些上下文相比，有一个非常明显的特点，它只提供了最基本的IoC容器的功能。从它的名字中可以看出，这个IoC容器可以读取以XML形式定义的BeanDefinition。理解这一点有助于我们理解ApplicationContext与基本的BeanFactory之间的区别和联系。我们可以认为直接的BeanFactory实现是IoC容器的基本形式，而各种ApplicationContext的实现是IoC容器的高级表现形式。

仔细阅读XmlBeanFactory的源码，在一开始的注释里面已经对 XmlBeanFactory的功能做了简要的说明，从代码的注释还可以看到，这是Rod Johnson在2001年就写下的代码，可见这个类应该是Spring的元老类了。它是继承DefaultListableBeanFactory这个类的，这个DefaultListableBeanFactory就是一个很值得注意的容器！
```java
/**
 * Convenient base class for {@link org.springframework.context.ApplicationContext}
 * implementations, drawing configuration from XML documents containing bean definitions
 * understood by an {@link org.springframework.beans.factory.xml.XmlBeanDefinitionReader}.
 *
 * <p>Subclasses just have to implement the {@link #getConfigResources} and/or
 * the {@link #getConfigLocations} method. Furthermore, they might override
 * the {@link #getResourceByPath} hook to interpret relative paths in an
 * environment-specific fashion, and/or {@link #getResourcePatternResolver}
 * for extended pattern resolution.
 */
public abstract class AbstractXmlApplicationContext extends AbstractRefreshableConfigApplicationContext {

	private boolean validating = true;
	public AbstractXmlApplicationContext() {
	}
	public AbstractXmlApplicationContext(ApplicationContext parent) {
		super(parent);
	}
	public void setValidating(boolean validating) {
		this.validating = validating;
	}

	/**
	 * Loads the bean definitions via an XmlBeanDefinitionReader.
	 */
	@Override
	protected void loadBeanDefinitions(DefaultListableBeanFactory beanFactory) throws BeansException, IOException {
		// Create a new XmlBeanDefinitionReader for the given BeanFactory.
		XmlBeanDefinitionReader beanDefinitionReader = new XmlBeanDefinitionReader(beanFactory);

		// Configure the bean definition reader with this context's
		// resource loading environment.
		beanDefinitionReader.setEnvironment(this.getEnvironment());
		
		//这里设置 XmlBeanDefinitionReader， 为XmlBeanDefinitionReader 配置ResourceLoader
		beanDefinitionReader.setResourceLoader(this);
		beanDefinitionReader.setEntityResolver(new ResourceEntityResolver(this));

		// Allow a subclass to provide custom initialization of the reader,
		// then proceed with actually loading the bean definitions.
		//这是启动Bean定义信息载入的过程
		initBeanDefinitionReader(beanDefinitionReader);
		loadBeanDefinitions(beanDefinitionReader);
	}

	/**
	 * Initialize the bean definition reader used for loading the bean
	 * definitions of this context. Default implementation is empty.
	 * <p>Can be overridden in subclasses, e.g. for turning off XML validation
	 * or using a different XmlBeanDefinitionParser implementation.
	 */
	protected void initBeanDefinitionReader(XmlBeanDefinitionReader reader) {
		reader.setValidating(this.validating);
	}

	/**
	 * Load the bean definitions with the given XmlBeanDefinitionReader.
	 * <p>The lifecycle of the bean factory is handled by the {@link #refreshBeanFactory}
	 * method; hence this method is just supposed to load and/or register bean definitions.

	 */
	protected void loadBeanDefinitions(XmlBeanDefinitionReader reader) throws BeansException, IOException {
		Resource[] configResources = getConfigResources();
		if (configResources != null) {
			reader.loadBeanDefinitions(configResources);
		}
		String[] configLocations = getConfigLocations();
		if (configLocations != null) {
			reader.loadBeanDefinitions(configLocations);
		}
	}

	protected Resource[] getConfigResources() {
		return null;
	}

}
```


刚看到，上面代码中使用 XmlBeanDefinitionReader来载入BeanDefinition到容器中，如以下代码：

```java
/**
	 * Load bean definitions from the specified XML file.
	 */
	@Override
	public int loadBeanDefinitions(Resource resource) throws BeanDefinitionStoreException {
		return loadBeanDefinitions(new EncodedResource(resource));
	}

	/**
	 * Load bean definitions from the specified XML file.
	 */
	//这里是载入XML形式的BeanDefinition。  
	public int loadBeanDefinitions(EncodedResource encodedResource) throws BeanDefinitionStoreException {
		Assert.notNull(encodedResource, "EncodedResource must not be null");
		if (logger.isInfoEnabled()) {
			logger.info("Loading XML bean definitions from " + encodedResource.getResource());
		}

		Set<EncodedResource> currentResources = this.resourcesCurrentlyBeingLoaded.get();
		if (currentResources == null) {
			currentResources = new HashSet<EncodedResource>(4);
			this.resourcesCurrentlyBeingLoaded.set(currentResources);
		}
		if (!currentResources.add(encodedResource)) {
			throw new BeanDefinitionStoreException(
					"Detected cyclic loading of " + encodedResource + " - check your import definitions!");
		}
		 //这里得到XML文件，并得到IO的InputSource准备进行读取。
		try {
			InputStream inputStream = encodedResource.getResource().getInputStream();
			try {
				InputSource inputSource = new InputSource(inputStream);
				if (encodedResource.getEncoding() != null) {
					inputSource.setEncoding(encodedResource.getEncoding());
				}
				//具体的读取过程可以在doLoadBeanDefinitions方法中找到：  
				return doLoadBeanDefinitions(inputSource, encodedResource.getResource());
			}
			finally {
				inputStream.close();
			}
		}
		catch (IOException ex) {
			throw new BeanDefinitionStoreException(
					"IOException parsing XML document from " + encodedResource.getResource(), ex);
		}
		finally {
			currentResources.remove(encodedResource);
			if (currentResources.isEmpty()) {
				this.resourcesCurrentlyBeingLoaded.remove();
			}
		}
	}
	
/**
	 * Actually load bean definitions from the specified XML file.
	 */
	//这是从特定的XML文件中实际载入BeanDefinition
	protected int doLoadBeanDefinitions(InputSource inputSource, Resource resource)
			throws BeanDefinitionStoreException {
		try {
			Document doc = doLoadDocument(inputSource, resource);
			return registerBeanDefinitions(doc, resource);
		}
		catch (BeanDefinitionStoreException ex) {
			throw ex;
		}
		catch (SAXParseException ex) {
			throw new XmlBeanDefinitionStoreException(resource.getDescription(),
					"Line " + ex.getLineNumber() + " in XML document from " + resource + " is invalid", ex);
		}
		catch (SAXException ex) {
			throw new XmlBeanDefinitionStoreException(resource.getDescription(),
					"XML document from " + resource + " is invalid", ex);
		}
		catch (ParserConfigurationException ex) {
			throw new BeanDefinitionStoreException(resource.getDescription(),
					"Parser configuration exception parsing XML from " + resource, ex);
		}
		catch (IOException ex) {
			throw new BeanDefinitionStoreException(resource.getDescription(),
					"IOException parsing XML document from " + resource, ex);
		}
		catch (Throwable ex) {
			throw new BeanDefinitionStoreException(resource.getDescription(),
					"Unexpected exception parsing XML document from " + resource, ex);
		}
	}

	/**
	 * Actually load the specified document using the configured DocumentLoader.
	 */
	//这里取得XML文件的Document对象，这个解析过程是由 documentLoader完成的，这个documentLoader是DefaultDocumentLoader,在定义documentLoader的地方创建
	protected Document doLoadDocument(InputSource inputSource, Resource resource) throws Exception {
		return this.documentLoader.loadDocument(inputSource, getEntityResolver(), this.errorHandler,
				getValidationModeForResource(resource), isNamespaceAware());
	}</span><pre name="code" class="java">/**
	 * Register the bean definitions contained in the given DOM document.
	 * Called by {@code loadBeanDefinitions}.
	 * <p>Creates a new instance of the parser class and invokes
	 * {@code registerBeanDefinitions} on it.
	 */
	//这里启动的是对BeanDefinition解析的详细过程，这个解析会使用到Spring的Bean配置规则，是我们下面需要详细关注的地方。 
	public int registerBeanDefinitions(Document doc, Resource resource) throws BeanDefinitionStoreException {
		BeanDefinitionDocumentReader documentReader = createBeanDefinitionDocumentReader();
		int countBefore = getRegistry().getBeanDefinitionCount();
		documentReader.registerBeanDefinitions(doc, createReaderContext(resource));
		return getRegistry().getBeanDefinitionCount() - countBefore;
	}
```
关于具体的Spring BeanDefinition的解析，是在BeanDefinitionParserDelegate中完成的。这个类里包含了各种Spring Bean定义规则的处理，感兴趣的同学可以仔细研究。我们举一个例子来分析这个处理过程，比如我们最熟悉的对Bean元素的处理是怎样完成的，也就是我们在XML定义文件中出现的<bean></bean>这个最常见的元素信息是怎样被处理的。在这里，我们会看到那些熟悉的BeanDefinition定义的处理，比如id、name、aliase等属性元素。把这些元素的值从XML文件相应的元素的属性中读取出来以后，会被设置到生成的BeanDefinitionHolder中去。这些属性的解析还是比较简单的。对于其他元素配置的解析，比如各种Bean的属性配置，通过一个较为复杂的解析过程，这个过程是由parseBeanDefinitionElement来完成的。解析完成以后，会把解析结果放到BeanDefinition对象中并设置到BeanDefinitionHolder中去，如以下所示：

```java
/**
	 * Parses the supplied {@code <bean>} element. May return {@code null}
	 * if there were errors during parse. Errors are reported to the
	 * {@link org.springframework.beans.factory.parsing.ProblemReporter}.
	 */
	public BeanDefinitionHolder parseBeanDefinitionElement(Element ele, BeanDefinition containingBean) {
		//这里取得在<bean>元素中定义的id、name和aliase属性的值
		String id = ele.getAttribute(ID_ATTRIBUTE);
		String nameAttr = ele.getAttribute(NAME_ATTRIBUTE);

		List<String> aliases = new ArrayList<String>();
		if (StringUtils.hasLength(nameAttr)) {
			String[] nameArr = StringUtils.tokenizeToStringArray(nameAttr, MULTI_VALUE_ATTRIBUTE_DELIMITERS);
			aliases.addAll(Arrays.asList(nameArr));
		}

		String beanName = id;
		if (!StringUtils.hasText(beanName) && !aliases.isEmpty()) {
			beanName = aliases.remove(0);
			if (logger.isDebugEnabled()) {
				logger.debug("No XML 'id' specified - using '" + beanName +
						"' as bean name and " + aliases + " as aliases");
			}
		}

		if (containingBean == null) {
			checkNameUniqueness(beanName, aliases, ele);
		}

		//这个方法会引发对bean元素的详细解析 
		AbstractBeanDefinition beanDefinition = parseBeanDefinitionElement(ele, beanName, containingBean);
		if (beanDefinition != null) {
			if (!StringUtils.hasText(beanName)) {
				try {
					if (containingBean != null) {
						beanName = BeanDefinitionReaderUtils.generateBeanName(
								beanDefinition, this.readerContext.getRegistry(), true);
					}
					else {
						beanName = this.readerContext.generateBeanName(beanDefinition);
						// Register an alias for the plain bean class name, if still possible,
						// if the generator returned the class name plus a suffix.
						// This is expected for Spring 1.2/2.0 backwards compatibility.
						String beanClassName = beanDefinition.getBeanClassName();
						if (beanClassName != null &&
								beanName.startsWith(beanClassName) && beanName.length() > beanClassName.length() &&
								!this.readerContext.getRegistry().isBeanNameInUse(beanClassName)) {
							aliases.add(beanClassName);
						}
					}
					if (logger.isDebugEnabled()) {
						logger.debug("Neither XML 'id' nor 'name' specified - " +
								"using generated bean name [" + beanName + "]");
					}
				}
				catch (Exception ex) {
					error(ex.getMessage(), ele);
					return null;
				}
			}
			String[] aliasesArray = StringUtils.toStringArray(aliases);
			return new BeanDefinitionHolder(beanDefinition, beanName, aliasesArray);
		}

		return null;
	}
```
在具体生成BeanDefinition以后。我们举一个对property进行解析的例子来完成对整个BeanDefinition载入过程的分析，还是在类BeanDefinitionParserDelegate的代码中，它对BeanDefinition中的定义一层一层地进行解析，比如从属性元素集合到具体的每一个属性元素，然后才是对具体的属性值的处理。根据解析结果，对这些属性值的处理会封装成PropertyValue对象并设置到BeanDefinition对象中去，如以下所示。
```java
/**
	 * Parse property sub-elements of the given bean element.
	 */
	/** 
	 * 这里对指定bean元素的property子元素集合进行解析。 
	 */ 
	public void parsePropertyElements(Element beanEle, BeanDefinition bd) {
		//遍历所有bean元素下定义的property元素  
		NodeList nl = beanEle.getChildNodes();
		for (int i = 0; i < nl.getLength(); i++) {
			Node node = nl.item(i);
			if (isCandidateElement(node) && nodeNameEquals(node, PROPERTY_ELEMENT)) {
				 //在判断是property元素后对该property元素进行解析的过程  
				parsePropertyElement((Element) node, bd);
			}
		}
	}

	/**
	 * Parse a property element.
	 */  
	public void parsePropertyElement(Element ele, BeanDefinition bd) {
		//这里取得property的名字  
		String propertyName = ele.getAttribute(NAME_ATTRIBUTE);
		if (!StringUtils.hasLength(propertyName)) {
			error("Tag 'property' must have a 'name' attribute", ele);
			return;
		}
		this.parseState.push(new PropertyEntry(propertyName));
		try {
			//如果同一个bean中已经有同名的存在，则不进行解析，直接返回。也就是说，如果在同一个bean中有同名的property设置，那么起作用的只是第一个。
			if (bd.getPropertyValues().contains(propertyName)) {
				error("Multiple 'property' definitions for property '" + propertyName + "'", ele);
				return;
			}
			//这里是解析property值的地方，返回的对象对应对Bean定义的property属性设置的解析结果，这个解析结果会封装到PropertyValue对象中，
			//然后设置到BeanDefinitionHolder中去。  
			Object val = parsePropertyValue(ele, bd, propertyName);
			PropertyValue pv = new PropertyValue(propertyName, val);
			parseMetaElements(ele, pv);
			pv.setSource(extractSource(ele));
			bd.getPropertyValues().addPropertyValue(pv);
		}
		finally {
			this.parseState.pop();
		}
	}

	/**
	 * Get the value of a property element. May be a list etc.
	 * Also used for constructor arguments, "propertyName" being null in this case.
	 */
	/** 
	 * 这里取得property元素的值，也许是一个list或其他。 
	 */  
	public Object parsePropertyValue(Element ele, BeanDefinition bd, String propertyName) {
		String elementName = (propertyName != null) ?
						"<property> element for property '" + propertyName + "'" :
						"<constructor-arg> element";

		// Should only have one child element: ref, value, list, etc.
		NodeList nl = ele.getChildNodes();
		Element subElement = null;
		for (int i = 0; i < nl.getLength(); i++) {
			Node node = nl.item(i);
			if (node instanceof Element && !nodeNameEquals(node, DESCRIPTION_ELEMENT) &&
					!nodeNameEquals(node, META_ELEMENT)) {
				// Child element is what we're looking for.
				if (subElement != null) {
					error(elementName + " must not contain more than one sub-element", ele);
				}
				else {
					subElement = (Element) node;
				}
			}
		}

		boolean hasRefAttribute = ele.hasAttribute(REF_ATTRIBUTE);
		boolean hasValueAttribute = ele.hasAttribute(VALUE_ATTRIBUTE);
		if ((hasRefAttribute && hasValueAttribute) ||
				((hasRefAttribute || hasValueAttribute) && subElement != null)) {
			error(elementName +
					" is only allowed to contain either 'ref' attribute OR 'value' attribute OR sub-element", ele);
		}

		if (hasRefAttribute) {
			String refName = ele.getAttribute(REF_ATTRIBUTE);
			if (!StringUtils.hasText(refName)) {
				error(elementName + " contains empty 'ref' attribute", ele);
			}
			RuntimeBeanReference ref = new RuntimeBeanReference(refName);
			ref.setSource(extractSource(ele));
			return ref;
		}
		else if (hasValueAttribute) {
			TypedStringValue valueHolder = new TypedStringValue(ele.getAttribute(VALUE_ATTRIBUTE));
			valueHolder.setSource(extractSource(ele));
			return valueHolder;
		}
		else if (subElement != null) {
			return parsePropertySubElement(subElement, bd);
		}
		else {
			// Neither child element nor "ref" or "value" attribute found.
			error(elementName + " must specify a ref or value", ele);
			return null;
		}
	}

	public void parseMetaElements(Element ele, BeanMetadataAttributeAccessor attributeAccessor) {
		NodeList nl = ele.getChildNodes();
		for (int i = 0; i < nl.getLength(); i++) {
			Node node = nl.item(i);
			if (isCandidateElement(node) && nodeNameEquals(node, META_ELEMENT)) {
				Element metaElement = (Element) node;
				String key = metaElement.getAttribute(KEY_ATTRIBUTE);
				String value = metaElement.getAttribute(VALUE_ATTRIBUTE);
				BeanMetadataAttribute attribute = new BeanMetadataAttribute(key, value);
				attribute.setSource(extractSource(metaElement));
				attributeAccessor.addMetadataAttribute(attribute);
				
				/**
				 * attributeAccessor.addMetadataAttribute(attribute);
				 * 最终调用下面方法
				 * name：attribute.getName(), 
				 * value：attribute
				 * 	
					public void setAttribute(String name, Object value) {
						Assert.notNull(name, "Name must not be null");
						if (value != null) {
							this.attributes.put(name, value);
						}
						else {
							removeAttribute(name);
						}
					}
				 * 
				 */
			}
		}
	}
```
比如，再往下看，我们看到像List这样的属性配置是怎样被解析的，依然在BeanDefinitionParserDelegate中：返回的是一个List对象，这个List是Spring定义的ManagedList，作为封装List这类配置定义的数据封装，如以下代码所示。
```java
	/**
	 * Parse a list element.
	 */
	public List<Object> parseListElement(Element collectionEle, BeanDefinition bd) {
		String defaultElementType = collectionEle.getAttribute(VALUE_TYPE_ATTRIBUTE);
		NodeList nl = collectionEle.getChildNodes();
		ManagedList<Object> target = new ManagedList<Object>(nl.getLength());
		target.setSource(extractSource(collectionEle));
		target.setElementTypeName(defaultElementType);
		target.setMergeEnabled(parseMergeAttribute(collectionEle));
		parseCollectionElements(nl, target, bd, defaultElementType);
		return target;
	}


	protected void parseCollectionElements(
			NodeList elementNodes, Collection<Object> target, BeanDefinition bd, String defaultElementType) {
		//遍历所有的元素节点，并判断其类型是否为Element。
		for (int i = 0; i < elementNodes.getLength(); i++) {
			Node node = elementNodes.item(i);
			if (node instanceof Element && !nodeNameEquals(node, DESCRIPTION_ELEMENT)) {
				//加入到target中去，target是一个ManagedList，同时触发对下一层子元素的解析过程，这是一个递归的调用。  
				target.add(parsePropertySubElement((Element) node, bd, defaultElementType));
			}
		}
	}
```
经过这样一层一层的解析，我们在XML文件中定义的BeanDefinition就被整个给载入到了IoC容器中，并在容器中建立了数据映射。在IoC容器中建立了对应的数据结构，或者说可以看成是POJO对象在IoC容器中的映像，这些数据结构可以以AbstractBeanDefinition为入口，让IoC容器执行索引、查询和操作。
这里的BeanDefinition的载入可以说是IoC容器的核心，如果说IoC容器是Spring的核心，那么这些BeanDefinition就是Spring的核心的核心了！

这一部分还是比较吃力，可以先了解一下基本原理，关于IOC XML配置可参考之前文章

[http://blog.csdn.net/u011229848/article/details/52845821](http://blog.csdn.NET/u011229848/article/details/52845821) [自己动手模拟spring的IOC](http://blog.csdn.NET/u011229848/article/details/52845821)

谢谢！