- Groovy脚本可以在ApplicationContext中配置，在TestContext框架中集成加载。
   - See the section called “Context configuration with Groovy scripts” for details.

- 通过TestTransaction API，测试管理事务可以通过编程开启和关闭测试方法。
   - See the section called “Programmatic transaction management” for details.
   
- 通过新的@Sql和@SqlConfig注释，在类和方法中使用可以配置声明。
   - See Section 15.5.8, “Executing SQL scripts” for details.
   
- 覆盖系统和应用的property sources的测试property sources可以通过新的@TestPropertySource来配置。
   - See the section called “Context configuration with test property sources” for details.
   
- 默认的TestExecutionListeners现在可以自动被加载。
   - See the section called “Automatic discovery of default TestExecutionListeners” for details.

- 自定义TestExecutionListeners可以自动和默认listeners合并。
   - See the section called “Merging TestExecutionListeners” for details.
   
- 在TestContext框架中有关事务测试支持的文档已经更加丰富，并有很多示例可以参考。
   - See Section 15.5.7, “Transaction management” for details.
   
- 对MockServletContext、MockHttpServletRequest和其他Servlet API也有一定的改进。

- AssertThrows现在已经支持Throable，而不是Exception。

- 在spring mvc测试，JSON的返回可以通过JSON Assert作为一个额外的选择，使用JSONPath，和对XML和XMLUnit类似。

- MockMvcBuilder现在可以通过MockMvcConfigurer的帮助创建。这是为了使得应用spring安全框架更加简单，可以压缩在使用第三方框架的步骤。

- MockRestServiceServer现在支持为了客户端测试的AsyncRestTemplate。