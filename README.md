import networkx as nx
import matplotlib.pyplot as plt

G = nx.DiGraph()
G.add_node("Data", pos=(-2500, 700))
G.add_node("Code", pos=(-4200, 0))
G.add_node("Inferences", pos=(-1000, 0))
G.add_node("Parameters", pos=(-2500, -700))
G.add_node("Updates", pos=(1500, 0))
G.add_node("Transparency", pos=(4000, 0))

G.add_edges_from([("Data", "Inferences")])
G.add_edges_from([("Code", "Inferences")])
G.add_edges_from([("Parameters", "Inferences")])
G.add_edges_from([("Inferences", "Updates")])
G.add_edges_from([("Updates", "Transparency")])

pos = nx.get_node_attributes(G, 'pos')
labels = {"Inferences": "Inferences",
          "Data": "Data",
          "Code": "Code",
          "Parameters": "Parameters",
          "Updates": "Updates",
          "Transparency": "Transparency"}  # Added label for "NDI" node in the labels dictionary

# Update color for the "Scenarios" node
node_colors = ["lightblue","lightblue", "lavender", "lightblue", "lightblue", "lightblue"]
# node_colors = ["lightblue","lavender", "lavender", "lightgreen", "lightpink", "lightpink"]
# Suppress the deprecation warning
import warnings
warnings.filterwarnings("ignore", category=DeprecationWarning)

plt.figure(figsize=(10, 8))
nx.draw(G, pos, with_labels=False, node_size=20000, node_color=node_colors, linewidths=2, edge_color='black', style='solid')
nx.draw_networkx_labels(G, pos, labels, font_size=14) # , font_weight='bold'
nx.draw_networkx_edges(G, pos, edge_color='black', style='solid', width=2)
plt.xlim(-5000, 5000)
plt.ylim(-1000, 1000)
plt.axis("off")
plt.show()

