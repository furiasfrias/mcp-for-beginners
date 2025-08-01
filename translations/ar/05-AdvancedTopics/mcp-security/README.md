<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "e363328861e6e00258187f731a773411",
  "translation_date": "2025-07-16T23:36:17+00:00",
  "source_file": "05-AdvancedTopics/mcp-security/README.md",
  "language_code": "ar"
}
-->
# أفضل ممارسات الأمان

الأمان أمر حيوي في تطبيقات MCP، خاصة في بيئات المؤسسات. من المهم التأكد من حماية الأدوات والبيانات من الوصول غير المصرح به، وتسريبات البيانات، وغيرها من التهديدات الأمنية.

## مقدمة

يتطلب بروتوكول سياق النموذج (MCP) اعتبارات أمنية محددة بسبب دوره في توفير وصول نماذج اللغة الكبيرة (LLMs) إلى البيانات والأدوات. تستعرض هذه الدرس أفضل ممارسات الأمان لتطبيقات MCP بناءً على إرشادات MCP الرسمية وأنماط الأمان المعتمدة.

يتبع MCP مبادئ أمنية رئيسية لضمان تفاعلات آمنة وموثوقة:

- **موافقة المستخدم والتحكم**: يجب أن يقدم المستخدمون موافقة صريحة قبل الوصول إلى أي بيانات أو تنفيذ أي عمليات. وفر تحكمًا واضحًا في البيانات التي يتم مشاركتها والإجراءات المصرح بها.
  
- **خصوصية البيانات**: يجب الكشف عن بيانات المستخدم فقط بموافقته الصريحة ويجب حمايتها بواسطة ضوابط وصول مناسبة. احمِ البيانات من النقل غير المصرح به.
  
- **سلامة الأدوات**: قبل استدعاء أي أداة، يجب الحصول على موافقة صريحة من المستخدم. يجب أن يكون لدى المستخدم فهم واضح لوظيفة كل أداة، ويجب فرض حدود أمان قوية.

## أهداف التعلم

بنهاية هذا الدرس، ستكون قادرًا على:

- تنفيذ آليات مصادقة وتفويض آمنة لخوادم MCP.
- حماية البيانات الحساسة باستخدام التشفير والتخزين الآمن.
- ضمان تنفيذ آمن للأدوات مع ضوابط وصول مناسبة.
- تطبيق أفضل الممارسات لحماية البيانات والامتثال للخصوصية.
- التعرف على نقاط الضعف الأمنية الشائعة في MCP مثل مشكلة الوكيل المشوش، تمرير الرموز، واختطاف الجلسات، والتعامل معها.

## المصادقة والتفويض

المصادقة والتفويض ضروريان لتأمين خوادم MCP. المصادقة تجيب على سؤال "من أنت؟" بينما التفويض يجيب على "ماذا يمكنك أن تفعل؟".

وفقًا لمواصفات أمان MCP، هذه اعتبارات أمنية حاسمة:

1. **التحقق من الرموز**: يجب ألا تقبل خوادم MCP أي رموز لم تصدر صراحةً لخادم MCP. "تمرير الرموز" هو نمط مضاد ممنوع صراحة.

2. **التحقق من التفويض**: يجب على خوادم MCP التي تنفذ التفويض التحقق من جميع الطلبات الواردة ويجب ألا تستخدم الجلسات للمصادقة.

3. **إدارة الجلسات الآمنة**: عند استخدام الجلسات للحالة، يجب على خوادم MCP استخدام معرفات جلسة آمنة وغير حتمية يتم إنشاؤها باستخدام مولدات أرقام عشوائية آمنة. يجب ربط معرفات الجلسة بمعلومات خاصة بالمستخدم.

4. **موافقة المستخدم الصريحة**: بالنسبة لخوادم الوكيل، يجب على تطبيقات MCP الحصول على موافقة المستخدم لكل عميل مسجل ديناميكيًا قبل إعادة التوجيه إلى خوادم التفويض الخارجية.

لننظر إلى أمثلة على كيفية تنفيذ المصادقة والتفويض الآمن في خوادم MCP باستخدام .NET وJava.

### تكامل الهوية في .NET

يوفر ASP .NET Core Identity إطارًا قويًا لإدارة مصادقة وتفويض المستخدمين. يمكننا دمجه مع خوادم MCP لتأمين الوصول إلى الأدوات والموارد.

هناك بعض المفاهيم الأساسية التي يجب فهمها عند دمج ASP.NET Core Identity مع خوادم MCP وهي:

- **تكوين الهوية**: إعداد ASP.NET Core Identity مع أدوار المستخدمين والمطالبات. المطالبة هي معلومة عن المستخدم، مثل دوره أو أذوناته، على سبيل المثال "Admin" أو "User".
- **مصادقة JWT**: استخدام رموز الويب JSON (JWT) للوصول الآمن إلى API. JWT هو معيار لنقل المعلومات بأمان بين الأطراف ككائن JSON، يمكن التحقق منه والوثوق به لأنه موقع رقميًا.
- **سياسات التفويض**: تعريف سياسات للتحكم في الوصول إلى أدوات محددة بناءً على أدوار المستخدمين. يستخدم MCP سياسات التفويض لتحديد من يمكنه الوصول إلى أي أدوات بناءً على أدوارهم ومطالباتهم.

```csharp
public class SecureMcpStartup
{
    public void ConfigureServices(IServiceCollection services)
    {
        // Add ASP.NET Core Identity
        services.AddIdentity<ApplicationUser, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext>()
            .AddDefaultTokenProviders();
        
        // Configure JWT authentication
        services.AddAuthentication(options =>
        {
            options.DefaultAuthenticateScheme = JwtBearerDefaults.AuthenticationScheme;
            options.DefaultChallengeScheme = JwtBearerDefaults.AuthenticationScheme;
        })
        .AddJwtBearer(options =>
        {
            options.TokenValidationParameters = new TokenValidationParameters
            {
                ValidateIssuer = true,
                ValidateAudience = true,
                ValidateLifetime = true,
                ValidateIssuerSigningKey = true,
                ValidIssuer = Configuration["Jwt:Issuer"],
                ValidAudience = Configuration["Jwt:Audience"],
                IssuerSigningKey = new SymmetricSecurityKey(
                    Encoding.UTF8.GetBytes(Configuration["Jwt:Key"]))
            };
        });
        
        // Add authorization policies
        services.AddAuthorization(options =>
        {
            options.AddPolicy("CanUseAdminTools", policy =>
                policy.RequireRole("Admin"));
                
            options.AddPolicy("CanUseBasicTools", policy =>
                policy.RequireAuthenticatedUser());
        });
        
        // Configure MCP server with security
        services.AddMcpServer(options =>
        {
            options.ServerName = "Secure MCP Server";
            options.ServerVersion = "1.0.0";
            options.RequireAuthentication = true;
        });
        
        // Register tools with authorization requirements
        services.AddMcpTool<BasicTool>(options => 
            options.RequirePolicy("CanUseBasicTools"));
            
        services.AddMcpTool<AdminTool>(options => 
            options.RequirePolicy("CanUseAdminTools"));
    }
    
    public void Configure(IApplicationBuilder app)
    {
        // Use authentication and authorization
        app.UseAuthentication();
        app.UseAuthorization();
        
        // Use MCP server middleware
        app.UseMcpServer();
    }
}
```

في الكود السابق، قمنا بـ:

- تكوين ASP.NET Core Identity لإدارة المستخدمين.
- إعداد مصادقة JWT للوصول الآمن إلى API. حددنا معلمات التحقق من الرموز، بما في ذلك المصدر، الجمهور، ومفتاح التوقيع.
- تعريف سياسات التفويض للتحكم في الوصول إلى الأدوات بناءً على أدوار المستخدمين. على سبيل المثال، سياسة "CanUseAdminTools" تتطلب أن يكون للمستخدم دور "Admin"، بينما سياسة "CanUseBasic" تتطلب أن يكون المستخدم مصادقًا.
- تسجيل أدوات MCP مع متطلبات تفويض محددة، لضمان أن المستخدمين الذين لديهم الأدوار المناسبة فقط يمكنهم الوصول إليها.

### تكامل Spring Security في Java

بالنسبة لـ Java، سنستخدم Spring Security لتنفيذ مصادقة وتفويض آمن لخوادم MCP. يوفر Spring Security إطار أمان شامل يتكامل بسلاسة مع تطبيقات Spring.

المفاهيم الأساسية هنا هي:

- **تكوين Spring Security**: إعداد تكوينات الأمان للمصادقة والتفويض.
- **خادم موارد OAuth2**: استخدام OAuth2 للوصول الآمن إلى أدوات MCP. OAuth2 هو إطار تفويض يسمح للخدمات الخارجية بتبادل رموز الوصول للوصول الآمن إلى API.
- **اعتراضات الأمان**: تنفيذ اعتراضات أمان لفرض ضوابط الوصول على تنفيذ الأدوات.
- **التحكم في الوصول بناءً على الدور**: استخدام الأدوار للتحكم في الوصول إلى أدوات وموارد محددة.
- **تعليقات الأمان**: استخدام التعليقات لتأمين الطرق ونقاط النهاية.

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
            .csrf().disable()
            .authorizeRequests()
                .antMatchers("/mcp/discovery").permitAll() // Allow tool discovery
                .antMatchers("/mcp/tools/**").hasAnyRole("USER", "ADMIN") // Require authentication for tools
                .antMatchers("/mcp/admin/**").hasRole("ADMIN") // Admin-only endpoints
                .anyRequest().authenticated()
            .and()
            .oauth2ResourceServer().jwt();
    }
    
    @Bean
    public McpSecurityInterceptor mcpSecurityInterceptor() {
        return new McpSecurityInterceptor();
    }
}

// MCP Security Interceptor for tool authorization
public class McpSecurityInterceptor implements ToolExecutionInterceptor {
    @Autowired
    private JwtDecoder jwtDecoder;
    
    @Override
    public void beforeToolExecution(ToolRequest request, Authentication authentication) {
        String toolName = request.getToolName();
        
        // Check if user has permissions for this tool
        if (toolName.startsWith("admin") && !authentication.getAuthorities().contains("ROLE_ADMIN")) {
            throw new AccessDeniedException("You don't have permission to use this tool");
        }
        
        // Additional security checks based on tool or parameters
        if ("sensitiveDataAccess".equals(toolName)) {
            validateDataAccessPermissions(request, authentication);
        }
    }
    
    private void validateDataAccessPermissions(ToolRequest request, Authentication auth) {
        // Implementation to check fine-grained data access permissions
    }
}
```

في الكود السابق، قمنا بـ:

- تكوين Spring Security لتأمين نقاط نهاية MCP، مع السماح بالوصول العام لاكتشاف الأدوات مع طلب المصادقة لتنفيذ الأدوات.
- استخدام OAuth2 كخادم موارد للتعامل مع الوصول الآمن إلى أدوات MCP.
- تنفيذ اعتراض أمان لفرض ضوابط الوصول على تنفيذ الأدوات، بالتحقق من أدوار المستخدم وأذوناته قبل السماح بالوصول إلى أدوات محددة.
- تعريف التحكم في الوصول بناءً على الدور لتقييد الوصول إلى أدوات الإدارة والوصول إلى البيانات الحساسة بناءً على أدوار المستخدمين.

## حماية البيانات والخصوصية

حماية البيانات أمر حاسم لضمان التعامل الآمن مع المعلومات الحساسة. يشمل ذلك حماية المعلومات الشخصية القابلة للتعريف (PII)، والبيانات المالية، وغيرها من المعلومات الحساسة من الوصول غير المصرح به والتسريبات.

### مثال حماية البيانات في Python

لننظر إلى مثال على كيفية تنفيذ حماية البيانات في Python باستخدام التشفير واكتشاف PII.

```python
from mcp_server import McpServer
from mcp_tools import Tool, ToolRequest, ToolResponse
from cryptography.fernet import Fernet
import os
import json
from functools import wraps

# PII Detector - identifies and protects sensitive information
class PiiDetector:
    def __init__(self):
        # Load patterns for different types of PII
        with open("pii_patterns.json", "r") as f:
            self.patterns = json.load(f)
    
    def scan_text(self, text):
        """Scans text for PII and returns detected PII types"""
        detected_pii = []
        # Implementation to detect PII using regex or ML models
        return detected_pii
    
    def scan_parameters(self, parameters):
        """Scans request parameters for PII"""
        detected_pii = []
        for key, value in parameters.items():
            if isinstance(value, str):
                pii_in_value = self.scan_text(value)
                if pii_in_value:
                    detected_pii.append((key, pii_in_value))
        return detected_pii

# Encryption Service for protecting sensitive data
class EncryptionService:
    def __init__(self, key_path=None):
        if key_path and os.path.exists(key_path):
            with open(key_path, "rb") as key_file:
                self.key = key_file.read()
        else:
            self.key = Fernet.generate_key()
            if key_path:
                with open(key_path, "wb") as key_file:
                    key_file.write(self.key)
        
        self.cipher = Fernet(self.key)
    
    def encrypt(self, data):
        """Encrypt data"""
        if isinstance(data, str):
            return self.cipher.encrypt(data.encode()).decode()
        else:
            return self.cipher.encrypt(json.dumps(data).encode()).decode()
    
    def decrypt(self, encrypted_data):
        """Decrypt data"""
        if encrypted_data is None:
            return None
        
        decrypted = self.cipher.decrypt(encrypted_data.encode())
        try:
            return json.loads(decrypted)
        except:
            return decrypted.decode()

# Security decorator for tools
def secure_tool(requires_encryption=False, log_access=True):
    def decorator(cls):
        original_execute = cls.execute_async if hasattr(cls, 'execute_async') else cls.execute
        
        @wraps(original_execute)
        async def secure_execute(self, request):
            # Check for PII in request
            pii_detector = PiiDetector()
            pii_found = pii_detector.scan_parameters(request.parameters)
            
            # Log access if required
            if log_access:
                tool_name = self.get_name()
                user_id = request.context.get("user_id", "anonymous")
                log_entry = {
                    "timestamp": datetime.now().isoformat(),
                    "tool": tool_name,
                    "user": user_id,
                    "contains_pii": bool(pii_found),
                    "parameters": {k: "***" for k in request.parameters.keys()}  # Don't log actual values
                }
                logging.info(f"Tool access: {json.dumps(log_entry)}")
            
            # Handle detected PII
            if pii_found:
                # Either encrypt sensitive data or reject the request
                if requires_encryption:
                    encryption_service = EncryptionService("keys/tool_key.key")
                    for param_name, pii_types in pii_found:
                        # Encrypt the sensitive parameter
                        request.parameters[param_name] = encryption_service.encrypt(
                            request.parameters[param_name]
                        )
                else:
                    # If encryption not available but PII found, you might reject the request
                    raise ToolExecutionException(
                        "Request contains sensitive data that cannot be processed securely"
                    )
            
            # Execute the original method
            return await original_execute(self, request)
        
        # Replace the execute method
        if hasattr(cls, 'execute_async'):
            cls.execute_async = secure_execute
        else:
            cls.execute = secure_execute
        return cls
    
    return decorator

# Example of a secure tool with the decorator
@secure_tool(requires_encryption=True, log_access=True)
class SecureCustomerDataTool(Tool):
    def get_name(self):
        return "customerData"
    
    def get_description(self):
        return "Accesses customer data securely"
    
    def get_schema(self):
        # Schema definition
        return {}
    
    async def execute_async(self, request):
        # Implementation would access customer data securely
        # Since we used the decorator, PII is already detected and encrypted
        return ToolResponse(result={"status": "success"})
```

في الكود السابق، قمنا بـ:

- تنفيذ فئة `PiiDetector` لفحص النص والمعلمات بحثًا عن المعلومات الشخصية القابلة للتعريف (PII).
- إنشاء فئة `EncryptionService` للتعامل مع تشفير وفك تشفير البيانات الحساسة باستخدام مكتبة `cryptography`.
- تعريف مزين `secure_tool` الذي يلف تنفيذ الأداة لفحص PII، وتسجيل الوصول، وتشفير البيانات الحساسة إذا لزم الأمر.
- تطبيق مزين `secure_tool` على أداة نموذجية (`SecureCustomerDataTool`) لضمان التعامل الآمن مع البيانات الحساسة.

## مخاطر الأمان الخاصة بـ MCP

وفقًا لوثائق أمان MCP الرسمية، هناك مخاطر أمنية محددة يجب أن يكون مطورو MCP على دراية بها:

### 1. مشكلة الوكيل المشوش

تحدث هذه الثغرة عندما يعمل خادم MCP كوكيل لواجهات برمجة التطبيقات الخارجية، مما قد يسمح للمهاجمين باستغلال العلاقة الموثوقة بين خادم MCP وهذه الواجهات.

**التخفيف:**
- يجب على خوادم الوكيل في MCP التي تستخدم معرفات عملاء ثابتة الحصول على موافقة المستخدم لكل عميل مسجل ديناميكيًا قبل إعادة التوجيه إلى خوادم التفويض الخارجية.
- تنفيذ تدفق OAuth الصحيح مع PKCE (إثبات المفتاح لتبادل الرمز) لطلبات التفويض.
- التحقق بدقة من عناوين URI لإعادة التوجيه ومعرفات العملاء.

### 2. ثغرات تمرير الرموز

تحدث ثغرة تمرير الرموز عندما يقبل خادم MCP رموزًا من عميل MCP دون التحقق من أن الرموز صادرة بشكل صحيح لخادم MCP ويمررها إلى واجهات برمجة التطبيقات اللاحقة.

### المخاطر
- تجاوز ضوابط الأمان (تجاوز تحديد المعدل، التحقق من الطلبات)
- مشاكل المساءلة ومسار التدقيق
- انتهاكات حدود الثقة
- مخاطر التوافق المستقبلي

**التخفيف:**
- يجب ألا تقبل خوادم MCP أي رموز لم تصدر صراحةً لخادم MCP.
- تحقق دائمًا من مطالبات جمهور الرموز لضمان تطابقها مع الخدمة المتوقعة.

### 3. اختطاف الجلسة

يحدث هذا عندما يحصل طرف غير مصرح له على معرف الجلسة ويستخدمه لانتحال هوية العميل الأصلي، مما قد يؤدي إلى إجراءات غير مصرح بها.

**التخفيف:**
- يجب على خوادم MCP التي تنفذ التفويض التحقق من جميع الطلبات الواردة ويجب ألا تستخدم الجلسات للمصادقة.
- استخدم معرفات جلسة آمنة وغير حتمية يتم إنشاؤها باستخدام مولدات أرقام عشوائية آمنة.
- اربط معرفات الجلسة بمعلومات خاصة بالمستخدم باستخدام تنسيق مفتاح مثل `<user_id>:<session_id>`.
- نفذ سياسات انتهاء صلاحية وتدوير الجلسات بشكل صحيح.

## ممارسات أمان إضافية لـ MCP

بعيدًا عن اعتبارات الأمان الأساسية في MCP، فكر في تنفيذ هذه الممارسات الأمنية الإضافية:

- **استخدم HTTPS دائمًا**: قم بتشفير الاتصال بين العميل والخادم لحماية الرموز من الاعتراض.
- **نفذ التحكم في الوصول بناءً على الدور (RBAC)**: لا تتحقق فقط مما إذا كان المستخدم مصادقًا؛ بل تحقق مما هو مخول له القيام به.
- **المراقبة والتدقيق**: سجل جميع أحداث المصادقة لاكتشاف والاستجابة للنشاط المشبوه.
- **التعامل مع تحديد المعدل والحد من الطلبات**: نفذ تراجعًا أسيًا ومنطق إعادة المحاولة للتعامل مع حدود المعدل بسلاسة.
- **تخزين الرموز بأمان**: خزّن رموز الوصول وتجديد الرموز بأمان باستخدام آليات التخزين الآمن للنظام أو خدمات إدارة المفاتيح الآمنة.
- **فكر في استخدام إدارة API**: يمكن لخدمات مثل Azure API Management التعامل مع العديد من مخاوف الأمان تلقائيًا، بما في ذلك المصادقة، التفويض، وتحديد المعدل.

## المراجع

- [MCP Security Best Practices](https://modelcontextprotocol.io/specification/draft/basic/security_best_practices)
- [MCP Authorization Specification](https://modelcontextprotocol.io/specification/draft/basic/authorization)
- [MCP Core Concepts](https://modelcontextprotocol.io/docs/concepts/architecture)
- [OAuth 2.0 Security Best Practices (RFC 9700)](https://datatracker.ietf.org/doc/html/rfc9700)

## ما التالي

- [5.9 Web search](../web-search-mcp/README.md)

**إخلاء المسؤولية**:  
تمت ترجمة هذا المستند باستخدام خدمة الترجمة الآلية [Co-op Translator](https://github.com/Azure/co-op-translator). بينما نسعى لتحقيق الدقة، يرجى العلم أن الترجمات الآلية قد تحتوي على أخطاء أو عدم دقة. يجب اعتبار المستند الأصلي بلغته الأصلية المصدر الموثوق به. للمعلومات الهامة، يُنصح بالاعتماد على الترجمة البشرية المهنية. نحن غير مسؤولين عن أي سوء فهم أو تفسير ناتج عن استخدام هذه الترجمة.