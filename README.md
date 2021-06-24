# made by Cyclops
my opcode patterns (dats from may 2021) :
make your own handlers idgaf
```c++
	template <class T>
	struct handlerRegister final { // reg
		static_assert(std::is_function<T>::value || std::is_constructible<std::string, T>::value, "need a string/function handler.");
	private:
		T handler;
	public:
		std::string_view pattern{};
		constexpr auto getHandler() const noexcept {
			return this->handler;
		};
        handlerRegister(const std::string_view&& s, T&& handler) noexcept : pattern{ std::move(s) }, handler(std::forward<T>(handler)) { }; //move the rvalue outside
        ~handlerRegister() = default;
	};
  
  	inline std::map<int, stringPattern> opcodeStringPatterns = { //could've made it a std::set but shrug
        {ARTIMETIC_INSTRUCTION, { R"(\b(L_\d+_\[L_\d+_(?:\[L_\d+_\])?\])\s=\s([L_\d\[\]\s]+(?=([+*\/\-%])))\3\s(L_\d+_\[L_\d+_(?:\[L_\d+_\])?\])[\r\n;])", "$1=artimetic_instr_handler($2,\'$3\',$4)" }},
        {LEN_INSTRUCTION, { R"((L_\d+_\[L_\d+_(?:\[L_\d+_\])?\])\s*?=\s*?#(L_\d+_\[L_\d+_(?:\[L_\d+_\])?\])[\r\n;])", "$1=len_handler($2)"}},
        {CONCAT_INSTRUCTION, { R"(\bfor\s*?L_\d+_forvar\d+\s*?=\s*?L_\d+_\s*?(?:\+\s*?1)?,\s*?(?:L_\d+_\[L_\d+_(?:\[L_\d+_\])?\])\s*?do\s+(L_\d+_)\s*?=\s*?\1\s\.{2}\s*?L_\d+_\[L_\d+_forvar\d+\][;]?\s+end[;\r\n])", "figure it yourself idiot" }},
        {GETTABLE_INSTRUCTION, { R"(\b(L_\d+_\[L_\d+_(?:\[L_\d+_\])?\])\s*?=\s*?(L_\d+_\[L_\d+_(?:\[L_\d+_\])?\])\[(L_\d+_\[L_\d+_(?:\[L_\d+_\])?\])\][\r\n;])", "$1=gettable_handler($2,$3)" }},
        {SETTABLE_INSTRUCTION, { R"(\b(L_\d+_\[L_\d+_\[L_\d+_(?:\[L_\d+_\])?\]\])\[(L_\d+_\[L_\d+_(?:\[L_\d+_\])?\])\]\s*?=\s*?(L_\d+_\[L_\d+_(?:\[L_\d+_\])?\]))", "figure it yourself idiot" }},
        {CALL_INSTRUCTION, { R"((L_\d+_\[L_\d+_(?:\[L_\d+_\])?\])\((?:(L_\d+_\[L_\d+_\s?\+\s?1\])?|(L_\d+_\(L_\d+_,\s*?L_\d+_\s*?\+\s*?1,\s*?(?:L_\d+_\[L_\d+_(?:\[L_\d+_\])?\]))?)\))", "todo" }},
        {COMPARE_INSTRUCTION, { R"(\bif\s*?\((L_\d+_\[L_\d+_\[L_\d+_(?:\[L_\d+_\])?\]\])\s*?(~=|==|>|<|>=|<=)\s*?(L_\d+_\[L_\d+_\[L_\d+_(?:\[L_\d+_\])?\]\])\)\s*?then\s+L_\d+_\s*?=\s*?L_\d+_\[L_\d+_\][\r\n;]\s*?end)", "figure it yourself idiot" }},
        {IF_STMT_INSTRUCTION, { R"(\bif\s*?\((L_\d+_\[L_\d+_(?:\[L_\d+_\])?\])\)\s*?then\s+L_\d+_\s*?=\s*?L_\d+_\[L_\d+_\][;\r\n]\s*?end)", "figure it yourself idiot" }},
        {NOT_STMT_INSTRUCTION, { R"(\bif\s*?\(not\((L_\d+_\[L_\d+_(?:\[L_\d+_\])?\])\)\)\s*?then\s+L_\d+_\s*?=\s*?L_\d+_\[L_\d+_\][;\r\n]\s*?end)", "figure it yourself idiot" }}
	};
  
```
