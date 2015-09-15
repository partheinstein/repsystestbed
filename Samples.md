# Samples #

**Example 1 - Simple experiment for PeerTrust**
Suppose you want to test PeerTrust algorithm. It takes a Feedback History Graph (FHG) and outputs  Reputation Graph (RG). This flow can be shown using the  Petri Net below:

![http://repsystestbed.googlecode.com/files/PT_PetriNet.png](http://repsystestbed.googlecode.com/files/PT_PetriNet.png)

Sample code:
```
PetriNet workflow = new PetriNet(new PetriNetEdgeFactory());

FHG fhg0 = new FHG(new FeedbackHistoryEdgeFactory());
Place fhgPlace0 = new Place(fhg0);

PeerTrust pt0 = new PeerTrust();
Transition ptTransition0 = new Transition(pt0);
ptTransition0.setWorkflow(workflow);

RG rg1 = new RG(new ReputationEdgeFactory());
Place rgPlace1 = new Place(rg1);						

//create the workflow
workflow.addEdge(fhgPlace0, ptTransition0, 1);
workflow.addEdge(ptTransition0, rgPlace1, 1);
		
//parse the feedbacks from the arff file
DefaultArffFeedbackGenerator feedbackGen = new DefaultArffFeedbackGenerator();
ArrayList<Feedback> feedbacks1 = (ArrayList<Feedback>) feedbackGen.generateHardcoded(arffFileName1 + ".arff");

//put the first token and fire pt
Token t1 = new Token(feedbacks1, fhgPlace0);
fhgPlace0.putToken(t1, true);
ptTransition0.fire();

```

PeerTrust extends from Algorithm class (abstract). As a result, it must implement `PetriNetElementIntf::ArrayList update(ArrayList<Token> tokens)`. The tokens contain the changes that occurred in their corresponding place. In the example above, the changes are feedbacks in FHG. The output is a set of changes that is to be reflected in the outgoing place. In this case, it is RG. Thus, PeerTrust returns a list of reputation graph edges:

```
@Override
	public ArrayList update(ArrayList<Token> tokens) throws Exception 
	{
		Token t = null;
		ArrayList<ReputationEdge> changes = new ArrayList<ReputationEdge>();
		
		//cheating. I should be using all tokens
		if(tokens!=null && tokens.size()>0)
		{
			t = tokens.get(0);
			if(t!=null)
			{
				FHG fhg = (FHG) ((Place) t.m_place).getGraph();
				for(Agent src : (Set<Agent>)fhg.vertexSet())
				{
					for(Agent sink : (Set<Agent>)fhg.vertexSet())
					{
						logger.debug("Calculating rep between " + src +  " and " + sink);
						double rep = calculateTrustScoreInternal(src, sink, new ArrayList<Agent>(), fhg);
						changes.add(new ReputationEdge(src, sink, rep));

					}
				}
			}
		}
		
		return changes;
		
	}
```

Notes:
  * The testbed checks the type of the elements in the return list to determine whether the output place can update itself. For example, a list of reputation edges output by PeerTrust is passed as input to outgoing place RG with which it can update itself.
  * Both Algorithm and Graph implements `ArrayList update(ArrayList<Token> tokens)`.