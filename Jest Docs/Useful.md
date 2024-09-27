
- When conditionally calling the original function on a mock, just imitate the original behavior to avoid recursive calling

``` ts
const useInitialisedContextMock = jest.spyOn(contexts, "useInitialisedContext").mockImplementation((context: React.Context<any>) => {
	if (context === ProjectContext) {
		return {projectId: "testProjectId"};
	}
	return React.useContext(context);
});
```

- Call `mockRestore`, to ensure other tests in the same describe block don't get the same mock