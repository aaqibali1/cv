
/**
 * Class: KarelCompiledEngine
 * --------------------------
 * This class is in charge of compiling a piece of Karel
 * code into some abstraction such that it can execute 
 * the program one step at a time. Implements the same
 * interface as the karelEvalEngine.
 */
function KarelCompiler(karel) {

   var that = {};
   that.vm = new KarelVM(karel);

   that.compile = function(text) {
      var parser = new KarelParser();
      parser.setInput(text);
      var functions = [];
      var functionNames = [];
      while (true) {
         var token = parser.nextToken();
         if (token == "") break;
         parser.saveToken(token);
         var fn = parser.readFunction();
         functions.push(fn);
         functionNames.push(fn[1]);
      }
      that.vm.setUserFnNames(functionNames);
      for(var i = 0; i < functions.length; i++) {
         var fn = functions[i];
         var code = [];
         that.vm.resetTempCounter();
         that.vm.compile(fn[2], code);
         code.push(new ReturnIns());
         that.vm.functions[fn[1]] = code;
      }
      that.vm.reset();
      that.vm.startCheck();
   }

   that.executeStep = function() {
      var vm = that.vm;
      if (!vm.cf) return true;
      var running = true;
      while (running) {
         if (vm.atStatementBoundary()) running = false;
         vm.step();
      }
      return false;
   }

   return that;

}
